# bunch of undefined things in crypto/blake2b, shirou/gopsutil/disk, shirou/gopsutil/mem
NOT_FOR_ARCHS=		i386

COMMENT=		cluster scheduler

VERSION=		2.0.0
PKGNAME=		nomad-${VERSION}

DIST_TUPLE +=		github hashicorp nomad v${VERSION} .

DISTNAME=		nomad-${VERSION}
ALL_TARGET=		github.com/hashicorp/nomad

CATEGORIES=		sysutils

HOMEPAGE=		https://www.nomadproject.io

MAINTAINER=		Antoine Jacoutot <ajacoutot@openbsd.org>

# BSL-1.1 (https://www.hashicorp.com/bsl)
PERMIT_PACKAGE=		Yes

WANTLIB += c pthread

MODULES=		lang/go

MAKE_ENV += CGO_ENABLED=1
MAKE_ENV += GOBIN="${MODGO_WORKSPACE}/bin"
MAKE_ENV += GOMODCACHE="${MODGO_WORKSPACE}/pkg/mod"

USE_GMAKE = Yes

post-extract:
	cd ${WRKDIST} && \
	export GOMODCACHE=${MODGO_WORKSPACE}/pkg/mod && \
	go mod tidy && go mod vendor
	cp ${FILESDIR}/*.go ${WRKDIST}/vendor/github.com/hashicorp/consul/sdk/freeport

do-build:
	echo "huy ${WRKSRC}"
	cd ${WRKSRC} && ${MAKE_ENV} gmake -f GNUmakefile dev
	mkdir -p ${WRKSRC}/bin && cp ${MODGO_WORKSPACE}/bin/nomad ${WRKSRC}/bin/nomad

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/bin/nomad ${PREFIX}/bin/nomad

post-install:
	${INSTALL_DATA_DIR} ${PREFIX}/share/examples/nomad
	${SUBST_CMD} -c -m 0640 ${FILESDIR}/nomad.hcl \
		${PREFIX}/share/examples/nomad/nomad.hcl
	${INSTALL_DATA} ${WRKSRC}/terraform/shared/config/nomad_client.hcl \
		${PREFIX}/share/examples/nomad/
	${INSTALL_DATA} ${WRKSRC}/terraform/shared/config/nomad.hcl \
		${PREFIX}/share/examples/nomad/nomad_server.hcl

.include <bsd.port.mk>
