PORTNAME=	STAR
DISTVERSION=	2.7.11a
CATEGORIES=	biology
PKGNAMEPREFIX=	rna-

MAINTAINER=	jwb@FreeBSD.org
COMMENT=	Spliced Transcripts Alignment to a Reference
WWW=		https://github.com/alexdobin/STAR

LICENSE=	GPLv3
LICENSE_FILE=	${WRKDIR}/STAR-${DISTVERSION}/LICENSE

NOT_FOR_ARCHS=		armv6 armv7 i386 mips powerpc powerpcspe
NOT_FOR_ARCHS_REASON=	Requires 64-bit processor
.if !exists(/usr/include/omp.h)
BROKEN=		requires OpenMP support that is missing on this architecture
.endif

BUILD_DEPENDS=	xxd:sysutils/xxd
LIB_DEPENDS=	libhts.so:biology/htslib

USES=		compiler:c++11-lang gmake localbase:ldflags
# FIXME: When built with clang, number of input reads = 0
USE_GCC=	yes
USE_GITHUB=	yes
GH_ACCOUNT=	alexdobin

WRKSRC_SUBDIR=	source
MAKE_FLAGS=	HTSLIB=''

PLIST_FILES=	bin/STAR

post-patch:
	${RM} -rf ${WRKSRC}/htslib
	${REINPLACE_CMD} \
		-e 's|"htslib/htslib/sam.h"|<htslib/sam.h>|g' \
		-e 's|"htslib/htslib/kstring.h"|<htslib/kstring.h>|g' \
		-e 's|"htslib/htslib/bgzf.h"|<htslib/bgzf.h>|g' \
		${WRKSRC}/BAMfunctions.cpp \
		${WRKSRC}/IncludeDefine.h \
		${WRKSRC}/STAR.cpp \
		${WRKSRC}/bamRemoveDuplicates.cpp \
		${WRKSRC}/bam_cat.c \
		${WRKSRC}/bam_cat.h \
		${WRKSRC}/signalFromBAM.h

.include <bsd.port.options.mk>

.if ${ARCH} != amd64
MAKE_FLAGS+=	CXXFLAGS_SIMD=""
.endif

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/STAR ${STAGEDIR}${PREFIX}/bin

.include <bsd.port.mk>
