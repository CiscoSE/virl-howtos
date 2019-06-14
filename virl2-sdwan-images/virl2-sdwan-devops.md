# How to add SD-WAN nodes to CML2

## Node Definitions
In the top right, click Tools -> Node and Image Definitions. Scroll down to the Nodes category. You will be prompted to import a definition. Click Browse and select each file provided in the "Node Definitions" folder.

## Image Upload and Definitions
Note that the version numbers in the examples below will vary depending on the release you choose to deploy in your environment.

In the top right, click 'Tools' -> 'Node and Image Definitions.' Click 'Manage Uploaded Images'. Click 'Browse' and upload an image for each of the following:
- csr1000v-ucmk9.16.12.2r-serial.qcow2
- viptela-edge-19.2.1-genericx86-64.qcow2
- viptela-smart-19.2.1-genericx86-64.qcow2
- viptela-vmanage-19.2.1-genericx86-64.qcow2

Please note that you will have to upload the vEdge image twice due to CML only allowing a single image definition per image. This has to be done only after attaching the first vEdge image to it's respective image definition (vBond or vEdge).

Once those have been uploaded you will click on 'Create New Image Definition.' Please create the following image definitions:

### IOS-XE
ID: iosxe-sdwan-16.12.2r

Label: IOS XE SD-WAN 16.12.2r

Description: ''

Disk Image: csr1000v-ucmk9.16.12.2r-serial.qcow2

Node Definition: XE-SDWAN

### vBond
ID: viptela-bond-19.2.1

Label: vBond 19.2.1

Description: ''

Disk Image: viptela-edge-19.2.1-genericx86-64.qcow2

Node Definition: vBond


### vEdge
ID: viptela-edge-19.2.1

Label: vEdge 19.2.1

Description: ''

Disk Image: viptela-edge-19.2.1-genericx86-64.qcow2

Node Definition: vEdge


### vManage
ID: viptela-manage-19.2.1

Label: vManage 19.2.1

Description: ''

Disk Image: viptela-vmanage-19.2.1-genericx86-64.qcow2

Node Definition: vManage


### vSmart
ID: viptela-smart-19.2.1

Label: vSmart 19.2.1

Description: ''

Disk Image: viptela-smart-19.2.1-genericx86-64.qcow2

Node Definition: vSmart
