# tensorXFV
Author: Caesar Wiratama

## For agents

| | |
|--|--|
| **Pick when** | Vehicle external aero (car / truck / similar) |
| **Also consider** | General external → `tensorXF` |
| **Combine with** | Crash/drop via OpenRadioss if needed |
| **Load** | README, `system/*`, `0.orig/*`, mesh scripts |
| **Parent** | [`../README.md`](../README.md) |

TensorCFD is a project developed by pt-tensor.com to help accelerate the deployment of OpenFOAM technology in real-world industrial applications. 
It consists of a collection of templates for specific applications. You simply copy and paste the template, then replace it with your own geometry or boundary conditions!

TensorXFV is a part of the Tensor CFD project, designed for Vehicle external flow aerodynamics, incompressible, steady, isothermal, with RAS turbulent model to simulate aircraft, UAV, road vehicles, buildings, sporting goods, and any other external flow cases.
Please include credit to "tensorXFV" in your work as acknowledgment if you use this template folder.

Author: Caesar Wiratama

DISCLAIMER
This is a development version, some setups might not be optimised yet for accuracy, validity, or computational efficiency.
The validity and accuracy of your results depend on your specific case and setup. pt-tensor.com assumes no responsibility for any results produced using this template.

How to Run:
#1. Make the script files executable: execute 'chmod +x buildMesh', 'chmod +x Run', 'chmod +x cleanResults', 'chmod +x Allclean', 'chmod +x Allrun'
#2. build the mesh: execute './buildMesh'
#3. Run the simulation: execute './Run'
#4. Clean the results: execute './cleanResults'
#5. Optional all-in-one: './Allrun' (= './buildMesh' then './Run'). Wipe mesh + results with './Allclean'.

How to Update Geometry:
#1. Export your vehicle as an STL and place it at constant/triSurface/object.stl. Keep the filename object.stl, or change the filename in system/snappyHexMeshDict and system/surfaceFeatureExtractDict to match.
#2. Align the geometry with flow: X = streamwise, Y = vertical (ground near y = 0), Z = spanwise. Size the background domain in system/blockMeshDict (vertices and cell counts) so the vehicle sits inside the box with enough space upstream, downstream, above, and to the sides.
#3. In system/snappyHexMeshDict: keep geometry { object.stl ... name truck_body; }, resize refinementBox_* min/max around the vehicle and near wake, and set locationInMesh to a point in the fluid (not inside the solid). Adjust surface refinement levels if needed.
#4. You can also generate blockMeshDict and snappyHexMeshDict values with this web-based mesh generator: https://tensorcalculators.com/external-flow-blockmesh-and-snappyhexmesh-calculator/
#5. Rebuild the mesh: execute './buildMesh'

How to Update Boundary Conditions:
#1. Initial fields live in 0.orig/ and are restored after meshing. This template uses patches named inlet, outlet, lowerWall, upperWall, frontAndBack (from blockMesh) and truck_body / truck_bodyGroup (from snappyHexMesh). Keep those names, or update every file in 0.orig/ (and system/forceCoeffs) to match your new patch names.
#2. Set freestream speed in 0.orig/include/initialConditions (flowVelocity). The default is (40 0 0) m/s. Inlet U uses that value; also set the same speed in system/forceCoeffs (magUInf).
#3. truck_bodyGroup is a no-slip wall. lowerWall is a moving ground (fixedValue U = freestream). upperWall and frontAndBack are slip.
#4. Pressure and turbulence BCs are in 0.orig/p, 0.orig/k, 0.orig/omega, and 0.orig/nut. Keep wall functions on truck_bodyGroup and lowerWall unless you change the near-wall treatment.
#5. If you change speed or vehicle size, also update system/forceCoeffs (magUInf, lRef, Aref, CofR) so force coefficients stay consistent.

Validation Study:
[1] https://en.pt-tensor.com/ahmed-bluff-body-openfoam-validation-study-mesh-and-reynolds-number-sensitivity-studies/

Documentation:
1st release: October 24th, 2024 = basic external flow and snappyHexMesh
Update 1: February 20th, 2025 = Change to openfoam 2406 version, add some script files
Update 2: March 1st, 2025 = Create a new branch from TensorXF to TensorXFV which dedicated for ground vehicle aerodynamics
Update 3: September 1st, 2026 = tensorXFV 2026, update branding to pt-tensor.com, add geometry and boundary-condition instructions
Update 4: September 29th, 2026 = Update mesh and computational schemes with validated Ahmed Body benchmark case
