## Installation & Start

### Manual

```shell
sudo pip install -e .
sudo cp etc/isoflat.ini /etc/neutron/isoflat.ini
sudo sed -i '/^service_plugins/ s/$/,isoflat/' /etc/neutron/neutron.conf
sudo sed -i '/^\[agent\]/a extensions = isoflat' /etc/neutron/plugins/ml2/ml2_conf.ini
neutron net-create --shared --provider:physical_network public --provider:network_type flat provider
sudo neutron-server --config-file /etc/neutron/neutron.conf --config-file /etc/neutron/plugins/ml2/ml2_conf.ini --config-file /etc/neutron/isoflat.ini
/usr/bin/python /usr/local/bin/neutron-openvswitch-agent --config-file /etc/neutron/neutron.conf --config-file /etc/neutron/plugins/ml2/ml2_conf.ini --config-file /etc/neutron/isoflat.ini
```

/etc/neutron/plugins/ml2/ml2_conf.ini
```ini
[ml2_type_flat]
flat_networks = 
```

### DevStack

A `local.conf` recipe to enable isoflat:

    [[local|localrc]]
    enable_plugin isoflat https://github.com/ppoffice/isoflat
    enable_service isoflat
    ISOFLAT_SERVICE_DRIVER=ISOFLAT:ISOFLAT:neutron_isoflat.services.isoflat.service_drivers.isoflat_rpc.IsoflatRpcDriver:default
