# Inventor_API
This is a simple Windows Forms application demonstrating how to create a basic extruded feature in Autodesk Inventor using the Inventor API.

## Overview

The application consists of a Windows Form with two buttons:
- **Connect**: Attempts to connect to an existing instance of Autodesk Inventor. If Inventor is not running, it launches a new instance.
- **Extrude**: Creates a new part document in Inventor, adds a rectangle sketch, and extrudes it.

## Usage

1. Click the **Connect** button to start or connect to Autodesk Inventor.
2. Click the **Extrude** button to create a new part document, add a rectangle sketch, and extrude it.

## Requirements

- Windows operating system
- Autodesk Inventor installed

## Setup

1. Clone this repository to your local machine.
2. Open the solution in Visual Studio.
3. Build and run the application.

## Contributing

Contributions are welcome! If you'd like to improve this project, please fork the repository and submit a pull request.
## Sample code 
```c#

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using Inventor;

namespace WindowsFormsApp1
{
    public partial class Form1 : Form
    {
        Inventor.Application inventorApplication;

        public Form1()
        {
            InitializeComponent();
        }

        // Event handler for the "Connect" button
        private void Connect_Click(object sender, EventArgs e)
        {
            try
            {
                // Attempt to get an existing instance of the Inventor application
                inventorApplication = (Inventor.Application)System.Runtime.InteropServices.Marshal.GetActiveObject("Inventor.Application");

            }
            catch
            {
                // If an instance of Inventor is not found, create a new one
                Type invtype = System.Type.GetTypeFromProgID("Inventor.Application");
                inventorApplication = System.Activator.CreateInstance(invtype) as Inventor.Application;
                inventorApplication.Visible = true;
            }
        }

        // Event handler for the "Extrude" button
        private void Extrude_Click(object sender, EventArgs e)
        {
            // Create a new part document
            PartDocument oPartDoc = (PartDocument)inventorApplication.Documents.Add(DocumentTypeEnum.kPartDocumentObject, inventorApplication.FileManager.GetTemplateFile(DocumentTypeEnum.kPartDocumentObject));
            PartComponentDefinition oPartCompDef = oPartDoc.ComponentDefinition;
            PlanarSketch oSketch = oPartCompDef.Sketches.Add(oPartCompDef.WorkPlanes[3]);
            TransientGeometry transGeo = inventorApplication.TransientGeometry;

            // Add the rectangle sketch lines
            SketchLine oLine1 = oSketch.SketchLines.AddByTwoPoints(transGeo.CreatePoint2d(0, 0), transGeo.CreatePoint2d(4, 0));
            SketchLine oLine2 = oSketch.SketchLines.AddByTwoPoints(transGeo.CreatePoint2d(4, 0), transGeo.CreatePoint2d(4, 3));
            SketchLine oLine3 = oSketch.SketchLines.AddByTwoPoints(transGeo.CreatePoint2d(4, 3), transGeo.CreatePoint2d(0, 3));
            SketchLine oLine4 = oSketch.SketchLines.AddByTwoPoints(transGeo.CreatePoint2d(0, 3), transGeo.CreatePoint2d(0, 0));

            // Add a rectangle using a different method (redundant, consider removing)
            SketchEntitiesEnumerator oRectangle = oSketch.SketchLines.AddAsTwoPointRectangle(transGeo.CreatePoint2d(0, 0), transGeo.CreatePoint2d(4, 3));

            // Create a profile from the sketch
            Profile oProfile = oSketch.Profiles.AddForSolid();

            // Create an extrude feature from the profile
            ExtrudeDefinition ExtrudeDef = oPartCompDef.Features.ExtrudeFeatures.CreateExtrudeDefinition(oProfile, PartFeatureOperationEnum.kJoinOperation);
            ExtrudeDef.SetDistanceExtent(4, PartFeatureExtentDirectionEnum.kNegativeExtentDirection);
            ExtrudeFeature oExtrude = oPartCompDef.Features.ExtrudeFeatures.Add(ExtrudeDef);
        }
    }
}

```
## Output Screenshot
![image](https://github.com/AayushJoshiCCTECH/Inventor_API/assets/157628596/3d52b344-cf0d-4bff-9eb3-8153fb870884)

## Clone this repository to your local machine.

```
git clone https://github.com/AayushJoshiCCTECH/Inventor_API.git
```
