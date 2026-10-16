# Part 0. Assignment Breakdown

## Additional Notes by Jim
Remember, this project is a **solution for a client**, so if the client requests something, you need to keep it in mind!
When we toured the Workshop, Jim had a few requests and pieces of advice:
- Wants to store both the students ID and their email address
- On Materials
	- 3D Printer jobs need to specify colour
	- Laser cutting jobs need to specify Color, material (MDF, Plywood, Acrylic) and thickness
- If file uploads aren't implemented, then there should be a clear notice that students will need to email the job file to Jim (james.riley@ed.act.edu.au)

## Real Use Cases
- Gear Teeth Part
	- Small part and needs to be rigid, 100% infill please
	- Please print 3 of them so i have backups if it breaks
	- any PLA colour
- Adorable Rat
	- This is a small aesthetic component for my project. Please print hollow and light
	- Any colour PLA
- Boat
	- No specific colour
	- Boat for major project
- Gears
	- Laser cut 3 of Gear A, and 4 of Gear B (two separate files)
	- Material should be 3mm MDF

## Machine Details
Here's the table with all of our machines basic specs for the database! You don't need to have the ID's match this way, it doesn't matter which one has which ID, as long as it makes sense in your system!

| **machine_id** | **name**     | **type**      | **model**              | **working_dimensions** | **status** |
| -------------- | ------------ | ------------- | ---------------------- | ---------------------- | ---------- |
| **1**          | 3D Printer 1 | 3D Printing   | Creality Ender 3 V3 SE | 220 x 220 x 250 mm     | Available  |
| **2**          | 3D Printer 2 | 3D Printing   | Creality Ender 3 V3 SE | 220 x 220 x 250 mm     | Available  |
| **3**          | 3D Printer 3 | 3D Printing   | Creality Ender 3 V3 SE | 220 x 220 x 250 mm     | Available  |
| **4**          | Big Printer  | 3D Printing   | Creality K1 MAX        | 300 x 300 x 300 mm     | Available  |
| **5**          | Laser Cutter | Laser Cutting | CO2 Laser Cutter       | 600 x 300 mm           | Available  |

## Database Structure
Here's my ERD that I'll be working from for the project! Please enjoy the watermark so I will know if you just copy mine into your supporting document :)

![[Robotics Job Logger ERD.png]]

## Website Heirarchy
![[website_heirarchy.png]]