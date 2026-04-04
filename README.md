# Net diagramm

## Полезные ссылки

- [Документация](https://mermaid.js.org/syntax/entityRelationshipDiagram.html#layout)
- [Онлайн конвертер в графический формат](https://mermaid-ai-editor.com/mermaid_to_png.html)
- [Онлайн редактор](https://mermaid.live/edit#pako:eNqVVmtvmzAU_SvI-7JJENnmkYRvVdtNU59atK2aIkUITIIaHrIha5vmv8-Yp4lpskhJQJx77rnnXtvsgZ8GBLiA0KvIW1MvXiYa_2y8jKYvr0g7vBtGuteuF08RtDRX2yGIK8jOK7a5EgArgJ8mrNgqEWiZSHlMCeQc51ECpDxKBKoQ67_IyONMiTA4jzWQgyWkfSxHCZDkKBFt2bWgfXUnaHm8EcRvhuDXgo2frRihO0IlDBKYKsUoCAtQXcsoyhKoeB3nRpD6z4QeIw_VX_Xb9M91CXuJmvIWv1ffPOq9bXiFJN8QirEE75XIH-OqTjSftwl6aOeIXGhhuOVGI-jJpJJy-fDjukTvtl7yxD8SXJZSsh2raeH2SS1YQh-RYzV551droFGqNhZfH3nGsgUGS9I0i5I1T8VCfhtm2bZgZr8ZHY0icWX9Svi9BL9uL-61WopeXoiRx0vQBfayoM8_H2-_39986Qv7bxbcCcSDTvdp92oJjddNWtPUsQ312UzHGOoYQt2GWMdWeY0-UlCNwyhLydCUdKois2d5j7BmOtsYS1tcLYw7L0qMhzCMfKI93gwEdjWiqk5s8a_d1PtBroM8IZ3PzagNUkvjhaTgAXKsUaodo2aoW3g85cq0DXx0Irp4dSLc25BE2z9eTPYgWLE31LvGYPSVDZYWnn1WyMlRhfJkqZrdbgVV8CmrZwO82mu7M0UeTXz-YnGaA27FAmbkxItXtOBcF75PGBO7s8aLG41urT8vYCbVLKsubawXVbnyx1b8wFN_6zF2RUJNHiGW0_SZuJ_C9qxvcOKkaB7DEAIdrGkUADenBdFBTGjslbdAmL4EfGBisgQuvwxIWB75pYoDD8u85E-axk0kTYv1Briht2X8rsgCLyf1m1oLIUlA6GVaJDlwTduEggS4e_ACXMN08MSZmzPHmePpFPGNBLwC15o40IRTc464Q7ZtWgcdvIm0aALnU2RbU9Ox0Wxmc6cACaI8pXfVm6J4YTz8A0Dp47w)

## L2 схема сети

```mermaid

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

    gw1-tmp{
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
    ESXi06 :::esxi }|..o{ SW_CORE2 : vlanXXXX
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
        sfp-sfpplus2 SW_CORE2 "VLAN 33,250,88,220,200,240,100-199,1001-1002"
        sfp-sfpplus3 SW_Garazh "VLAN 33,220,200,100-199, 1001-1002"
        sfp-sfpplus4 SDS-Main-Office PK "VLAN 33,250,220,200,501,502,224,225,240,201,100-199, 1001-1002"
    }
    
    SW-CORE-SFP }|--o{ SDS-Main-Office : sfp-sfpplus1
    
    SDS-Main-Office {
        sfp-sfpplus1 dhcp_server
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
        sfp-sfpplus6 ESXi06 "Access vlan 200"
        sfp-sfpplus8 SW-CORE-SFP PK "VLAN 33,200,250,220,88,240,100-199,1001-1002"

    }

    classDef dhcp-snooping stroke:#f00
    classDef esxi stroke:#0f0

```
