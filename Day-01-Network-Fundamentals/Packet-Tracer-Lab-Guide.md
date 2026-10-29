# Day 01 — Packet Tracer Introduction Lab

> Companion guide for **Day 01 — Network Fundamentals & Network Devices**.
>
> **Packet Tracer project file:** `Day 01 Lab - Packet Tracer Introduction.pkt`

## Open the lab

1. Install and open Cisco Packet Tracer.
2. In Packet Tracer, select **File → Open**.
3. Select `Day 01 Lab - Packet Tracer Introduction.pkt`.
4. If you see a security or compatibility prompt, read it before proceeding.
5. Save your own working copy with **File → Save As** so the original remains unchanged.

## Packet Tracer basics

- **Select tool (arrow):** select or move a device or link.
- **Delete tool (red X):** click a device or link to remove it. If you accidentally activate Delete, press **Esc** or click the Select tool (arrow) to exit.
- **Undo:** try `Ctrl + Z` immediately after an accidental change; availability depends on the action/version.
- **Add a device:** choose a category at the bottom-left, then select the required device and click the workspace.
- **Connect devices:** choose the Connections (lightning bolt) category, select a cable type, then click each endpoint. Automatic connection can help beginners.
- **Inspect a device:** click it to view its configuration tabs and available tools.
- **Save:** use `Ctrl + S` regularly.

## First practice task

If the supplied topology includes PCs and a switch:

1. Identify each PC, switch, router, and any server.
2. Click each PC and inspect **Desktop → IP Configuration**.
3. Record the IP address and subnet mask for each PC.
4. Identify which devices are connected to the switch and which links connect to a router.
5. If two PCs are in the same subnet and VLAN, test connectivity from **Desktop → Command Prompt** with `ping <other-PC-IP>`.
6. If the ping fails, check the link status, IP address, subnet mask, VLAN and whether the other device is powered on.
7. Save a copy after making changes.

The exact tasks depend on the topology included in the `.pkt` file. Do not assume an IP address or configuration that is not shown in the lab.

## How to upload the .pkt file to this GitHub repository

Packet Tracer `.pkt` files are binary files. To add the actual lab file through GitHub's website:

1. Open the [Day 01 folder](https://github.com/zales2004/Networking/tree/main/Day-01-Network-Fundamentals).
2. Choose **Add file → Upload files**.
3. Select `Day 01 Lab - Packet Tracer Introduction.pkt` from your computer.
4. Commit the upload to the `main` branch.

Keep the filename consistent with the name shown above. This Markdown guide documents the lab; the `.pkt` file must appear separately in the folder before GitHub can host it as a downloadable project.

## Troubleshooting reminder

If you accidentally click the Delete tool and devices disappear, exit the tool with **Esc** or select the arrow tool. Immediately try `Ctrl + Z` to undo the last deletion. Save often, and use **Save As** to keep a clean original.
