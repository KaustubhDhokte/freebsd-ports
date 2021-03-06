# $FreeBSD$

PORTNAME=	ppsspp
DISTVERSIONPREFIX=	v
DISTVERSION?=	1.3
PORTREVISION?=	0
CATEGORIES=	emulators
# XXX Get from Debian once #697821 lands
MASTER_SITES=	https://bazaar.launchpad.net/~sergio-br2/${PORTNAME}/debian-sdl/download/5/${PORTNAME}.1-20140802045408-dd26dik367ztj5xg-8/:manpage
DISTFILES=	${PORTNAME}.1:manpage
EXTRACT_ONLY=	${DISTFILES:N*\:manpage:C/:.*//}

MAINTAINER=	jbeich@FreeBSD.org
COMMENT=	PSP emulator in C++ with dynarec JIT for x86, ARM, MIPS

LICENSE=	GPLv2+

LIB_DEPENDS=	libzip.so:archivers/libzip \
		libsnappy.so:archivers/snappy \
		libavcodec.so:multimedia/ffmpeg
RUN_DEPENDS=	xdg-open:devel/xdg-utils

USE_GITHUB=	yes
GH_ACCOUNT=	hrydgard:DEFAULT,lang Kingcom:ext_armips
GH_PROJECT=	${PORTNAME}-lang:lang armips:ext_armips
GH_TAGNAME?=	cdf4a8d:lang 1ffab37:ext_armips

CONFLICTS_INSTALL?=	${PORTNAME}-devel-[0-9]*

USES=		compiler:c++11-lib localbase pkgconfig
EXCLUDE=	glew snappy libzip zlib
EXTRACT_AFTER_ARGS=${EXCLUDE:S,^,--exclude ,}
USE_GL=		glew glu
USE_SDL=	sdl2 # joystick
CMAKE_ARGS=	-DUSE_SYSTEM_FFMPEG=on
QMAKE_SOURCE_PATH=	${WRKSRC}/Qt/PPSSPPQt.pro
QMAKE_ARGS=	CONFIG+="system_ffmpeg" LREL_TOOL="${LRELEASE}"
NO_INSTALL=	yes # ports/202949

DESKTOP_ENTRIES="PPSSPP" \
		"" \
		"${PORTNAME}" \
		"${PORTNAME} %f" \
		"Game;Emulator;" \
		""

OPTIONS_DEFAULT=QT5
OPTIONS_SINGLE=	GUI
OPTIONS_SINGLE_GUI=	QT4 QT5 SDL

QT4_USES=	qmake:outsource
QT4_USE=	QT4=qmake_build,moc_build,rcc_build,uic_build,linguisttools_build,gui,opengl
QT5_USES=	qmake:outsource
QT5_USE=	QT5=qmake_build,buildtools_build,linguisttools_build,gui,opengl,widgets
SDL_USES=	cmake:outsource ninja
SDL_BUILD_DEPENDS=	png>=1.6:graphics/png
SDL_LIB_DEPENDS=	libpng.so:graphics/png
SDL_VARS=	PORTDATA+="assets ${PORTNAME}"

post-extract:
# Convert USE_GITHUB groups to git-submodule(1) paths
.for group in ${GH_PROJECT:C/.*://}
	@${RMDIR} ${WRKSRC}/${group:S,_,/,g}
	@${MV} ${WRKSRC_${group}} ${WRKSRC}/${group:S,_,/,g}
.endfor

post-patch:
	@${REINPLACE_CMD} -e 's,/usr/share,${PREFIX}/share,' \
		${WRKSRC}/UI/NativeApp.cpp
	@${REINPLACE_CMD} -e 's,/usr,${LOCALBASE},' \
		${WRKSRC}/Qt/*.pr*
	@${REINPLACE_CMD} \
		-e 's/"unknown"/"${GH_TAGNAME}"/' \
		-e 's/^\(VERSION =\).*/\1 ${GH_TAGNAME}/' \
		${WRKSRC}/git-version.cmake \
		${WRKSRC}/Qt/Settings.pri

do-install-SDL-on:
# cmake doesn't pack assets into the app unlike qmake
# XXX Convert to ${PORTDATA:N${PORTNAME}} once fmake is EOL
	(cd ${INSTALL_WRKSRC} && ${COPYTREE_SHARE} \
		"${PORTDATA:Nppsspp}" ${STAGEDIR}${DATADIR})
# Install the app alongside assets to avoid warnings with GetExeDirectory()
	${INSTALL_PROGRAM} ${INSTALL_WRKSRC}/${PORTNAME:tu}* \
		${STAGEDIR}${DATADIR}/${PORTNAME}
	${LN} -sf ${DATADIR}/${PORTNAME} ${STAGEDIR}${PREFIX}/bin

do-install-SDL-off:
	${INSTALL_PROGRAM} ${INSTALL_WRKSRC}/${PORTNAME} \
		${STAGEDIR}${PREFIX}/bin

post-install:
	${INSTALL_MAN} ${_DISTDIR}/${PORTNAME}.1 \
		${STAGEDIR}${MAN1PREFIX}/man/man1
	(cd ${WRKSRC}/assets/unix-icons && ${COPYTREE_SHARE} \
		"hicolor" ${STAGEDIR}${PREFIX}/share/icons)
	${MKDIR} ${STAGEDIR}${PREFIX}/share/icons/hicolor/scalable/apps
	${INSTALL_DATA} ${WRKSRC}/assets/unix-icons/icon-512.svg \
		${STAGEDIR}${PREFIX}/share/icons/hicolor/scalable/apps/${PORTNAME}.svg

.include <bsd.port.mk>
