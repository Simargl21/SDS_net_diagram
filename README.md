# Net diagramm

## Полезные ссылки

- [Документация](https://mermaid.js.org/syntax/entityRelationshipDiagram.html#layout)
- [Онлайн конвертер в графический формат](https://mermaid-ai-editor.com/mermaid_to_png.html)
- [Онлайн редактор](https://mermaid.live/edit#pako:eNqVVmtvmzAU_SvI-7JJENnmkYRvVdtNU59atK2aIkUITIIaHrIha5vmv8-Yp4lpskhJQJx77rnnXtvsgZ8GBLiA0KvIW1MvXiYa_2y8jKYvr0g7vBtGuteuF08RtDRX2yGIK8jOK7a5EgArgJ8mrNgqEWiZSHlMCeQc51ECpDxKBKoQ67_IyONMiTA4jzWQgyWkfSxHCZDkKBFt2bWgfXUnaHm8EcRvhuDXgo2frRihO0IlDBKYKsUoCAtQXcsoyhKoeB3nRpD6z4QeIw_VX_Xb9M91CXuJmvIWv1ffPOq9bXiFJN8QirEE75XIH-OqTjSftwl6aOeIXGhhuOVGI-jJpJJy-fDjukTvtl7yxD8SXJZSsh2raeH2SS1YQh-RYzV551droFGqNhZfH3nGsgUGS9I0i5I1T8VCfhtm2bZgZr8ZHY0icWX9Svi9BL9uL-61WopeXoiRx0vQBfayoM8_H2-_39986Qv7bxbcCcSDTvdp92oJjddNWtPUsQ312UzHGOoYQt2GWMdWeY0-UlCNwyhLydCUdKois2d5j7BmOtsYS1tcLYw7L0qMhzCMfKI93gwEdjWiqk5s8a_d1PtBroM8IZ3PzagNUkvjhaTgAXKsUaodo2aoW3g85cq0DXx0Irp4dSLc25BE2z9eTPYgWLE31LvGYPSVDZYWnn1WyMlRhfJkqZrdbgVV8CmrZwO82mu7M0UeTXz-YnGaA27FAmbkxItXtOBcF75PGBO7s8aLG41urT8vYCbVLKsubawXVbnyx1b8wFN_6zF2RUJNHiGW0_SZuJ_C9qxvcOKkaB7DEAIdrGkUADenBdFBTGjslbdAmL4EfGBisgQuvwxIWB75pYoDD8u85E-axk0kTYv1Briht2X8rsgCLyf1m1oLIUlA6GVaJDlwTduEggS4e_ACXMN08MSZmzPHmePpFPGNBLwC15o40IRTc464Q7ZtWgcdvIm0aALnU2RbU9Ox0Wxmc6cACaI8pXfVm6J4YTz8A0Dp47w)

## L2 схема сети

```mermaid
---
title: Схема L2 соединений
config:
    layout: elk
---
erDiagram

    haproxy1 }|--o{ ESXi04 : v102
    vault1 }|--o{ ESXi04 : v100
    consul1 }|--o{ ESXi04 : v101

    haproxy3 }|--o{ ESXi06 : v102
    vault3 }|--o{ ESXi06 : v100
    consul3 }|--o{ ESXi06 : v101
    gw1-tmp }|--o{ ESXi06 : v101-1004

    haproxy2 }|--o{ ESXi05 : v102
    vault2 }|--o{ ESXi05 : v100
    consul2 }|--o{ ESXi05 : v101

    gw1-tmp:::router{
        v100-dmz-vault dhcp_server
        v101-dmz-consul dhcp_server
        v102-dmz-haproxy dhcp_server
        v104-dmz-mgmt-docker dhcp_server
    }
    
    ESXi04 :::esxi }|--o{ SW_Garazh : ether22
    ESXi04 {
        eth2 v100-199
    }

    ESXi06 :::esxi }|--o{ Servers2 : ether21
    ESXi06 :::esxi }|..o{ SW_CORE2 : vSwitch0_vmnic2_Access_vlan_200
    ESXi06 {
        ether21 v100-199
    }
    ESXi05 :::esxi }|--o{ Servers2 : ether22
    ESXi05 {
        ether22 v100-199
    }
    SW_Garazh }|--o{ SW-CORE-SFP:::dhcp-snooping : sfp-sfpplus3
    
    SW_Garazh {
        ether22 ESXi04_eth2 "VLAN 100-199, 1001-1002"
        sfp-sfpplus1(UPLINK) SW-CORE-SFP "VLAN 100-199, 1001-1002"
        sfp-sfpplus2 SW_Garazh2
    }

    SW-CORE-SFP {
        sfp-sfpplus1 Servers "VLAN 33,250,88,220,200,502,240,201"
        sfp-sfpplus2 SW_CORE2 PK "VLAN 33,250,88,220,200,240,100-199,1001-1002"
        sfp-sfpplus3 SW_Garazh "VLAN 33,220,200,100-199, 1001-1002"
        sfp-sfpplus4 SDS-Main-Office PK "VLAN 33,250,220,200,501,502,224,225,240,201,100-199, 1001-1002"
    }
    
    SW-CORE-SFP }|--o{ SDS-Main-Office : sfp-sfpplus1
    
    SDS-Main-Office }|--o{ ISP_1:::ISP : bridge_ISP1
    SDS-Main-Office }|--o{ ISP_2:::ISP : ether2
    SDS-Main-Office:::router {
        bridge_ISP1 ISP_1 "193.106.69.207/27"
        ether2 ISP_2 "91.228.118.11/29"
        sfp-sfpplus1 SW-CORE-SFTP
        ether11_bridge_ISP1 gw2_sds-team_ru
        ether12 gw2_sds-team_ru
    }

    gw2_sds-team_ru }o--o{ SDS-Main-Office : eth13-wan1
    gw2_sds-team_ru }o--o{ SDS-Main-Office : eth12-lnk-to-gw1
    gw2_sds-team_ru }|--o{ ISP_1 : eth13-wan1
    gw2_sds-team_ru }o--|{ SW_CORE : bridge_sfp-sfpplus7

    gw2_sds-team_ru:::router {
        eth13-wan1 ISP_1
        eth13-wan1 SDS-Main-Office
        eth12-lnk-to-gw1 SDS-Main-Office
        sfpplus1 SW_CORE
    }

    Servers }|--o{ SW-CORE-SFP : sfp-sfpplus1
    Servers {
        sfp-sfpplus1 SW-CORE-SFP
    }

    Servers2 }|--o{ SW_CORE2:::dhcp-snooping : sfp-sfpplus5
    Servers2 {
        ether21 ESXi06 "VLAN 100-199,1001-1002"
        ether22 ESXi05 "VLAN 100-199,1001-1002"
        sfp-sfpplus2 SW_CORE2 "VLAN 33,200,200,100-199,1001-1002"
    }

    SW_CORE2 }|--o{ SW-CORE-SFP : sfp-sfpplus8
    SW_CORE2 {
        sfp-sfpplus5 Servers2 PK "VLAN 33,220,100-199,1001-1002"
        sfp-sfpplus6 gw1-tmp_sds-team_ru "Access vlan 200"
        sfp-sfpplus6 ESXi06 "vSwitch0, vmnic2, Access vlan 200"
        sfp-sfpplus8 SW-CORE-SFP PK "VLAN 33,200,250,220,88,240,100-199,1001-1002"

    }

    SW_CORE }o--|{ SW_Garazh : bridge_sfp-sfpplus1
    SW_CORE }o--|{ SW_NewOffice : bridge_sfp-sfpplus4
    SW_CORE }o--|{ Servers2 : bridge_sfp-sfpplus2
    SW_CORE }o--|{ SW_CORE2 : bridge_sfp-sfpplus4
    SW_CORE }|--o{ SDS-Main-Office : bridge_sfp-sfpplus4
    SW_CORE:::dhcp-snooping {
        sfp-sfpplus1_bridge SW_Garazh "VLAN 22,200,220,100-199"
        sfp-sfpplus3_bridge SW_NewOffice "VLAN 33,88,220,240,250"
        sfp-sfpplus5_bridge Servers2 PK "VLAN 33,200,220,100-199"
        sfp-sfpplus6_bridge SW-CORE2 "VLAN 33,88,200,201,220,224,240,250,501,502,100-199"
        sfp-sfpplus7_bridge gw2_sds-team_ru "VLAN 33,250,220,88,240,100-199,1002"
        sfp-sfpplus8_bridge SDS-Main-Office PK "VLAN 33,88,200,201,220,224,240,250,501,502,100-199,1001"
    }

    classDef dhcp-snooping stroke:#f00
    classDef esxi stroke:#0f0
    classDef router fill:#f96
    classDef ISP fill:#f90
    
```
