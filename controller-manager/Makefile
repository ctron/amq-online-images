
ifeq (, $(shell which yq))
 $(error "Required dependency, yq, not found")
endif

LOCAL_ARTIFACT_DIR:=controller-manager/target/controller-manager-${VERSION}-dist.zip
OLM_VER := $(shell yq -r .channels[0].currentCSV  ../templates/install/olm/amq-online/amq-online.package.yaml | sed -e 's/^amqonline\.//')

include ../Makefile.common

build: modules/olm-manifest/build/olm_manifest.tar.gz modules/olm-manifest/build/olm_package.tar.gz

modules/olm-manifest/build/olm_manifest.tar.gz: $(wildcard ../templates/install/olm/amq-online/*.clusterserviceversion.yaml ../templates/install/olm/amq-online/*.crd.yaml)
	mkdir -p modules/olm-manifest/build
	tar czf $@ --transform 's,^,amq-online.$(OLM_VER)/,' -C $(<D) $(^F)
	tar tzf $@

modules/olm-manifest/build/olm_package.tar.gz: ../templates/install/olm/amq-online/amq-online.package.yaml
	mkdir -p modules/olm-manifest/build
	tar czf $@ -C $(<D) $(^F)
	tar tzf $@
