IMAGES += ppc64-rootfs.cpio.gz ppc64-novsx-rootfs.cpio.gz ppc64le-rootfs.cpio.gz
IMAGES += ppc-rootfs.cpio.gz
IMAGES += cloud-init-user-data.img
IMAGES += ubuntu16.04-cloudimg-powerpc.qcow2
IMAGES += ubuntu16.04-cloudimg-ppc64el.qcow2
IMAGES += ubuntu21.04-cloudimg-ppc64el.qcow2
IMAGES += ubuntu21.10-cloudimg-ppc64el.qcow2
IMAGES += fedora34-cloudimg-ppc64le.qcow2

all: $(IMAGES)

define wget
	wget -O $(1).tmp $(2)
	mv $(1).tmp $(1)
endef

define wget_lbt
	$(call wget,$(1),"https://github.com/groeck/linux-build-test/blob/236ccac32e5c3538f2c8221b359225cc40925ba0/rootfs/$(2)?raw=true")
endef

ppc64-rootfs.cpio.gz: ppc64-novsx-rootfs.cpio.gz
	ln -fs $< $@

ppc64-novsx-rootfs.cpio.gz:
	$(call wget_lbt,$@,ppc64/rootfs.cpio.gz)

ppc64le-rootfs.cpio.gz:
	$(call wget_lbt,$@,ppc64/rootfs-el.cpio.gz)

ppc-rootfs.cpio.gz:
	$(call wget_lbt,$@,ppc/rootfs.cpio.gz)

define wget_ubuntu
	$(call wget,$(1),"https://cloud-images.ubuntu.com/releases/$(2)/release/$(3)")
endef

ubuntu16.04-cloudimg-powerpc.qcow2:
	$(call wget_ubuntu,$@,xenial,ubuntu-16.04-server-cloudimg-powerpc-disk1.img)
	chmod a-w $@
	sha256sum $@ > $@.sum

ubuntu16.04-cloudimg-ppc64el.qcow2:
	$(call wget_ubuntu,$@,xenial,ubuntu-16.04-server-cloudimg-ppc64el-disk1.img)
	chmod a-w $@
	sha256sum $@ > $@.sum

ubuntu21.04-cloudimg-ppc64el.qcow2:
	$(call wget_ubuntu,$@,hirsute,ubuntu-21.04-server-cloudimg-ppc64el.img)
	chmod a-w $@
	sha256sum $@ > $@.sum

ubuntu21.10-cloudimg-ppc64el.qcow2:
	$(call wget_ubuntu,$@,impish,ubuntu-21.10-server-cloudimg-ppc64el.img)
	chmod a-w $@
	sha256sum $@ > $@.sum

fedora34-cloudimg-ppc64le.qcow2:
	$(call wget,$@,"https://download.fedoraproject.org/pub/fedora-secondary/releases/34/Cloud/ppc64le/images/Fedora-Cloud-Base-34-1.2.ppc64le.qcow2")
	chmod a-w $@
	sha256sum $@ > $@.sum

cloud-init-user-data.img: cloud-init-user-data.txt
	cloud-localds $@ $<

clean:
	@echo "Use distclean to remove rootfs images"

distclean:
	rm -f $(IMAGES) *.sum
