# $FreeBSD$

PORTNAME=	kdevelop-php-docs
PORTVERSION=	${KDEVELOP_VERSION:S/4./1./}
CATEGORIES=	devel kde
MASTER_SITES=	KDE/${KDEVELOP_BRANCH}/kdevelop/${KDEVELOP_VERSION}/src
DIST_SUBDIR=	KDE/kdevelop/${KDEVELOP_VERSION}

MAINTAINER=	kde@FreeBSD.org
COMMENT=	PHP documentation for KDevelop

LIB_DEPENDS=	libkdevplatforminterfaces.so:devel/kdevplatform

USES=		cmake:outsource compiler:c++11-lib kde:4 tar:bz2
USE_KDE=	kdelibs automoc4
USE_QT4=	moc_build qmake_build rcc_build uic_build

OPTIONS_DEFINE=	NLS
OPTIONS_SUB=	yes

NLS_USES=	gettext
NLS_CMAKE_OFF=	-DBUILD_po:BOOL=FALSE

.include <bsd.port.mk>
