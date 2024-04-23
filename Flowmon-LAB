#main.tf
provider "vsphere" {
  user = var.vsphere_user
  password = var.vsphere_password
  vsphere_server = var.vsphere_server
  allow_unverified_ssl = true
}
data "vsphere_datacenter" "dc" {
  name = "${var.vsphere_datacenter}"
}
data "vsphere_datastore" "datastore" {
  name          = "${var.vsphere_datastore}"
  datacenter_id = "${data.vsphere_datacenter.dc.id}"
}
data "vsphere_resource_pool" "pool" {
  name          = "${var.vsphere_resource_pool}"
  datacenter_id = "${data.vsphere_datacenter.dc.id}"
}
data "vsphere_virtual_machine" "template" {
  name          = "${var.vsphere_virtual_machine_template}"
  datacenter_id = "${data.vsphere_datacenter.dc.id}"
}
data "vsphere_network" "network0" {
  name          = "${var.vsphere_network0}"
  datacenter_id = "${data.vsphere_datacenter.dc.id}"
}
data "vsphere_network" "network1" {
  name          = "${var.vsphere_network2}"
  datacenter_id = "${data.vsphere_datacenter.dc.id}"
}
data "vsphere_network" "network2" {
  name          = "${var.vsphere_network2}"
  datacenter_id = "${data.vsphere_datacenter.dc.id}"
}

resource "vsphere_virtual_machine" "cloned_virtual_machine" {
  #for_each 		= var.vms
  count = var.POD-Count
  name = format("%s%02s%s",var.VM-name-prefix, count.index + 1,var.VM-name-posfix) #%02sT-t átírni %02s-re a véglegesben
  resource_pool_id = "${data.vsphere_resource_pool.pool.id}"
  datastore_id     = "${data.vsphere_datastore.datastore.id}"

  num_cpus = "${data.vsphere_virtual_machine.template.num_cpus}"
  memory   = "${data.vsphere_virtual_machine.template.memory}"
  guest_id = "${data.vsphere_virtual_machine.template.guest_id}"
  scsi_type = "${data.vsphere_virtual_machine.template.scsi_type}"
  wait_for_guest_net_timeout = 0

  network_interface {
    network_id   = "${data.vsphere_network.network0.id}"
    adapter_type = "${data.vsphere_virtual_machine.template.network_interface_types[0]}"
  }
  network_interface {
    network_id   = "${data.vsphere_network.network1.id}"
    adapter_type = "${data.vsphere_virtual_machine.template.network_interface_types[0]}"
  }
  network_interface {
    network_id   = "${data.vsphere_network.network2.id}"
    adapter_type = "${data.vsphere_virtual_machine.template.network_interface_types[0]}"
  }
  #firmware = var.boot #"efi"
  disk {
    label = "disk0"
    size = "${data.vsphere_virtual_machine.template.disks.0.size}"
  }
  disk {
    label = "disk1"
    size = "${data.vsphere_virtual_machine.template.disks.1.size}"
    unit_number = 1
  }
  folder = var.folder # hova rakja a vm-et
  clone {
    template_uuid = "${data.vsphere_virtual_machine.template.id}"
    linked_clone = true
  }
}
