# XXX no i386 equivalent to mem_openbsd_amd64.go
ONLY_FOR_ARCHS=		amd64,arm64

COMMENT=		service discovery and configuration tool

VERSION=		2.0.0
PKGNAME=		consul-${VERSION}

DIST_TUPLE +=		github hashicorp consul v${VERSION} .

DISTNAME=		consul-${VERSION}
ALL_TARGET=		github.com/hashicorp/consul

CATEGORIES=		sysutils net

HOMEPAGE=		https://www.consul.io/

MAINTAINER=		Antoine Jacoutot <ajacoutot@openbsd.org>

# BSL-1.1 (https://www.hashicorp.com/bsl)
PERMIT_PACKAGE=		Yes

WANTLIB += c kvm pthread

MODULES=		lang/go

# go/src/github.com/hashicorp/consul/agent/config/default.go
MODGO_LDFLAGS +=	-X github.com/hashicorp/consul/version.GitDescribe=${VERSION}
MODGO_LDFLAGS +=	-X github.com/hashicorp/consul/version.Version=${VERSION}
MODGO_LDFLAGS +=	-X github.com/hashicorp/consul/version.VersionPrerelease="" # empty

MAKE_ENV += CGO_ENABLED=1 
MAKE_ENV += GOBIN="${MODGO_WORKSPACE}/bin" 
MAKE_ENV += GOMODCACHE="${MODGO_WORKSPACE}/pkg/mod"

USE_GMAKE = Yes

post-extract:
	cd ${WRKDIR}/${PKGNAME} && \
	export GOMODCACHE=${MODGO_WORKSPACE}/pkg/mod && \
	go mod tidy && go mod vendor

do-build:
	cd ${WRKSRC} && ${MAKE_ENV} go install -ldflags "${MODGO_LDFLAGS}"
	mkdir -p ${WRKSRC}/bin && cp ${MODGO_WORKSPACE}/bin/consul ${WRKSRC}/bin/consul

SUBST_VARS +=		VERSION
post-patch:
	${SUBST_CMD} ${WRKDIST}/agent/uiserver/dist/index.html

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/bin/consul ${PREFIX}/bin/consul

post-install:
	${INSTALL_DATA_DIR} ${PREFIX}/share/examples/consul
	${SUBST_CMD} -c -m 0640 ${WRKSRC}/.release/linux/package/etc/consul.d/consul.hcl \
		${PREFIX}/share/examples/consul/consul.hcl

.include <bsd.port.mk>
