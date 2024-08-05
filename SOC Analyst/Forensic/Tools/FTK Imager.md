**File > Detect EFS Encryption**
To scan and detect the presence of encryption
#### Extract image
**File > Create Disk Image**
To create a forensic image
Check both
```text
Verify images after they are created
```
And 
```text
Create directory listings of all files in the image after they are created
```

Then **Add...** where so save the image in **raw/dd** format

#### Import image
**File > Add Evidence Item**
To add evidence to the pane, select **Image File** to use an extracted image or **Physical Drive** to work directly on the machine (not recommended)

On the evidence tree pane, right click on the root folder and **Export Files**

We now have access to the deleled files and the corrupted ones