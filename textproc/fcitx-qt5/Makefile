# Created by: Yuri Victorovich <yuri@rawbw.com>
# $FreeBSD$

PORTNAME=	fcitx-qt5
PORTVERSION=	1.0.5
CATEGORIES=	textproc

MAINTAINER=	yuri@rawbw.com
COMMENT=	Fcitx support for Qt5

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/COPYING

LIB_DEPENDS+=	libfcitx-config.so:chinese/fcitx \
		libQt5DBus.so:devel/dbus-qt5 \
		libxkbcommon.so:x11/libxkbcommon
BUILD_DEPENDS=	extra-cmake-modules>0:devel/extra-cmake-modules

USE_GITHUB=	yes
GH_ACCOUNT=	fcitx

USES=		compiler:c++11-lib cmake gettext pkgconfig
USE_QT5=	core gui qmake widgets buildtools_build
USE_LDCONFIG=	yes

.include <bsd.port.mk>
