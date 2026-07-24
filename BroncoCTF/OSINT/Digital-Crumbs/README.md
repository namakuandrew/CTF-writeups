# Digital Crumbs - WriteUp

Flag: bronco{3360}
____________________________________________________________________
## Description
My friends just sent me this picture of a coffee place they were at and told me to meet them at the pizza store across the street. What is the building number of that pizza shop?

Hint: Flag should be in the format `bronco{XXXX}`, where `XXXX` is the building number.
_____________________________________________________________________

# File Attachment
<p align="center">
  <img width="620" height="800" alt="image" src="https://github.com/user-attachments/assets/ed1eae0f-6862-46b8-8321-ec696b02ac3c" />
</p>

# 1. First Phase (Reconnaissance): Reverse search the image
Using the given image, we can do a reverse search using google lens to see the exact location of the cafe
<p align="center">
  <img width="1200" height="800" alt="image" src="https://github.com/user-attachments/assets/8a081765-1243-4025-a10e-cb63327b47fe" />
</p>

# 2. Second phase: Checking Google map
After finding the location of the cafe through reverse search, we can validate the location and also see the building number through google map
<p align="center">
  <img width="1545" height="911" alt="image" src="https://github.com/user-attachments/assets/46a1fb64-0c12-4764-b26d-e6b8f1aa30d6" />
</p>

______________________________________________________________________

# First Attemp
After finding the exact location of the cafe, i knew that the location of the pizza house must be adjacent to the cafe location and we can see this through google map that there is a pizza place across the street under the name Domino's pizza:
<p align="center">
 <img width="1162" height="722" alt="image" src="https://github.com/user-attachments/assets/104b2878-2df8-4fd1-923a-29f7f48dbbd7" />
</p>

While also knowing that the building number is present in the google map view, i try to enter the flag using the present building number
