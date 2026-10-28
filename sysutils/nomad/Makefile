# bunch of undefined things in crypto/blake2b, shirou/gopsutil/disk, shirou/gopsutil/mem
NOT_FOR_ARCHS=          i386

COMMENT=                cluster scheduler

VERSION=                2.0.7
PKGNAME=                nomad-${VERSION}

DIST_TUPLE +=           github hashicorp nomad v${VERSION} .

ALL_TARGET=             github.com/hashicorp/nomad

CATEGORIES=             sysutils net

HOMEPAGE=               https://www.nomadproject.io

# BSL-1.1 (https://www.hashicorp.com/bsl)
PERMIT_PACKAGE=         Yes

WANTLIB += c pthread

MODULES=                lang/go

USE_GMAKE = Yes

MODULES =               lang/go
MODGO_GOPATH =          ${MODGO_WORKSPACE}
MODGO_GO111MODULE =     auto
SUBST_VARS =            LOCALSTATEDIR
NO_TEST =               Yes

MAKE_ENV += CGO_ENABLED=1
MAKE_ENV += GOBIN="${MODGO_WORKSPACE}/bin"
MAKE_ENV += GOMODCACHE="${MODGO_WORKSPACE}/pkg/mod"

pre-patch:
        cd ${WRKDIST} && ${MAKE_ENV} GOPROXY=https://proxy.golang.org,direct go mod vendor

do-build:
        cd ${WRKSRC} && GOOS=openbsd ${MAKE_ENV} go install
        mkdir -p ${WRKSRC}/bin && cp ${MODGO_WORKSPACE}/bin/nomad ${WRKSRC}/bin/nomad

do-install:
        ${INSTALL_PROGRAM} ${WRKSRC}/bin/nomad ${PREFIX}/bin/nomad
        ${INSTALL_DATA_DIR} ${PREFIX}/share/examples/nomad
        ${SUBST_CMD} -c -m 0640 ${FILESDIR}/nomad.hcl \
                ${PREFIX}/share/examples/nomad/nomad.hcl
        ${INSTALL_DATA} ${WRKSRC}/terraform/shared/config/nomad_client.hcl \
                ${PREFIX}/share/examples/nomad/
        ${INSTALL_DATA} ${WRKSRC}/terraform/shared/config/nomad.hcl \
                ${PREFIX}/share/examples/nomad/nomad_server.hcl

.include <bsd.port.mk>