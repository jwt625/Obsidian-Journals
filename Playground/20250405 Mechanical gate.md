
2025-04-05T17:00:41-07:00
Reference:
- https://dl.acm.org/doi/10.1145/3025453.3025624

# Design & print

![[Pasted image 20250405170117.png]]

`pip install opencv-python gdspy numpy`
- already have these

2025-04-05T17:13:13-07:00
![[Pasted image 20250405171314.png]]
Edited the above image by hand.


![[Pasted image 20250405173852.png]]
![[Pasted image 20250405173845.png]]![[Pasted image 20250405174044.png]]

2025-04-05T18:17:11-07:00
Have to use this polygon handling option in the klayout dxf writer for openSCAD:
![[Pasted image 20250405181726.png]]

![[Pasted image 20250405181858.png]]
```

// Optionally, extrude the imported shape into a 3D object
linear_extrude(height = 100)
    import("output.dxf", convexity=10);

```


2025-04-05T18:25:15-07:00
Send the print and going to test two different size:
![[Pasted image 20250405182525.png]]

2025-04-05T19:27:22-07:00
Done making the box:
![[Pasted image 20250405192727.png]]
```
difference() {
    // Positive shape: extruded by 120 units
    linear_extrude(height = 130)
        import("box_pos.dxf", convexity=10);
    
    // Negative shape: extruded by 100 units and moved down by 1 unit
    translate([0, 0, 31])
    linear_extrude(height = 100)
        import("box_neg.dxf", convexity=10);
}

```


