# Ansible role to manage hostname.if files idempotent way
### Short example

   roles:
    - role: interfaces
      vars:
        ifaces:
          - if: "re0"
            family: "inet"
            ip: "dhcp"
            description: "Temporary uplink"

# Features:

* support all modern OpenBSD (as of July 01 2020) network interfaces
* idempotent
* is trying to adhere DRY principle strongly
* special processing options (link0, link1, link2) is given more mnemonical names, according to what they do

of course, one should still read corresponding manual pages in order to configure network correctly

# Interfaces
## Common interface options
following options are shared by nearly all interfaces
- role: interfaces
  vars:
    ifaces:
      - if: em0
        description: value
        ip: value
        family: value
        arp: value
        broadcast: value
        aliases:
          - family: value
            address: value
        groups:
         - group1
         - group2
        instance: value
        lladdr: value
        llprio: value
        media: value
        mediaopt: value
        metric: value
        mode: value
        mpls: value
        priority: value
        rdomain: value
        rtlabel: value
        staticarp: value
        wol: value
        state: value
 only if family is "inet6":
        autoconfprivacy: value
        anycast: value
        eui64: value
        pltime: value
        soii: value
        tentative: value
        vltime: value

## Vlans

vlan(4) interfaces are configred as follows:

roles:
    - role: interfaces
      vars:
        ifaces:
          - if: "vlan10"
            parent: "re0"
            vnetid: "10"
            ip: "192.168.2.1/24"

## Tunnels
All tunnel interfaces (egre|eoip|etherip|gif|gre|mgre|nvgre|vxlan) are added as follows:

- role: interfaces
      vars:
        ifaces:
          - if: "gif0"
            myside: "ip of my side"
            theirside: "ip of theirside"
            dest: "destination ip"
            ip: "192.168.2.1/32"

Also supported options  for tunnels are:
  keepalive: value
  rxprio: value
  tunneldf: value
  tunneldomain: value
  vnetflowid: value

## Wireless network devices
These are configured as follows:
- role: interfaces
      vars:
        ifaces:
          - if: iwn0
            bssid: "example"
            chan: "example"
            join: "example"
            nwflag: "example"
            nwid: "example"
            nwkey: "example"
            powersave: "y|n"
            wpaakms: "example"
            wpaciphers: "comma,separated"
            wpagroupcipher: "example"
            wpakey: "example"
            wpaprotos: "comma,separated"
            ip: "dhcp"

## Bridges
Are set with the  following options:

- role: interfaces
      vars:
        ifaces:
          - if: bridge0
            members:
              - em0
              - em1
            span:
              - em0
            autoedge:
              - em1
            autoptp:
              - em0
              - em1
            blocknonip:
              - em0
              - em1
            discover:
              - em0
              - em1
            edge:
              - em0
              - em1
            fwddelay: "value"
            hellotime: "value"
            holdcnt: "value"
            ifcost:
              - interface: "em0"
                cost: 15
              - interface: "em1"
                cost: 20
           ifpriority:
             - interface: "em0"
               priority: 60
           learn:
              - em0
           stop_ip_mutlicast: "y" #or link0: "y"
           stop_nonip_mutlicast: "y" #or link1: "y"
           pass_to_ipsec: "y" #or link2: "y"
           maxaddr: "value"
           maxage: "value"
           proto: "value"
           ptp:
            - em0
            - em1
           rules:
            - "block 121235677"
            - "pass"
           rulefile: "/etc/bridgerules"
           spanpriority: "value"
           staic:
             - interface: "em0"
               address: "someaddress"
           stp: em0
           timeout: 30
           state: up

## CARP
carp(4) specific options are:

- role: interfaces
  vars:
    ifaces:
     - if: carp0
       advbase: value
       advskew: value
       balancing: value
       carpnodes: "comma separated list of vhid:advskew"
       carpdev: value
       carppeer: value
       pass: value
       carpstate: "mapped to state"
       vhid: value

## MPLS
mpls(mpe|mpip|mpw) specific options are:

- role: interfaces
  vars:
    ifaces:
     - if: mpe0
       mpls-label: value
       pwecw: ""
       pwefat: ""
       neighbor: "value"
       neighbor-label: "value"
       tunneldomain: "value"

## PAIR
pair(4) specific options are:
- role: interfaces
  vars:
    ifaces:
      - if: pair0
        patch: pair1

## PFLOW
pflow(4) options are as follows:
- role: interfaces
  vars:
    ifaces:
     - if: plow0
       flowdst: "value"
       flowsrc: "value"
       pflowproto: "value"

## PFSYNC
pfsync(4) options are as follows:
- role: interfaces
  vars:
    ifaces:
    - if: pfsync
      defer: "y"
      maxupd: value
      syncdev: "em0"

## PPPOE
pppoe(4) options are as follows:
- role: interfaces
  vars:
    ifaces:
    - if: pppoe0
      authkey: "value"
      authname: "value"
      authproto: "value"
      peerflag:  "value"
      peerkey: "value"
      peername: "value"
      peerproto: "value"
      pppoeac: "value"
      pppoedev: "value"
      pppoesvc: "value"

## SWITCH
switch(4) options are as follows:
- role: interfaces
  vars:
    ifaces:
     - if: switch0
       members:
        - name: em0
          portno: 1
        - name: em1
          portno: 2
      local: "em0"
      datapath: "value"
      maxflow: "value"
      maxgroup: "value"
      protected:
         - name: em0
           domain_ids: "10,30"

## TRUNK
trunk(4) options are as follows:
- role: interfaces
  vars:
    ifaces:
      - if: trunk0
        members:
          - em1
          - em2
        lacpmode: value
        lacptimeout: value
        trunkproto: value

## UMB
- role: interfaces
  vars:
    ifaces:
      - if: umb0
        apn: value
        class: value
        pin: value
        roaming: "y"

## WIREGUARD
- role: interfaces
  vars:
    ifaces:
      - if: wg0
        wgkey: value
        wgport: value
        wgrtable: value
        wgpeer: value
        wgpsk: value
        wgpka: value
        wgendpoint:
           ip: value
           port: value
        wgaip: value
