# Jonathans-MSAA
Reshade shader mainly for older GPUs that don't have a lot of power for post proccessing.

Hey there! This shader works in ReShade, and may work other places that use HLSL! The rundown is that this uses luminance of a pixel to find surrounding pixels that are out of line and reduce jaggies. Designed for 1080p, the perfomance overhead may drastically increase above 1440p, though I haven't been able to test whether it does.

Install via ReShade's folder in the application you're running. On install, ReShade should create a reshade-shaders folder, so drag and drop and have fun!

Ver. 0.0.1 (This will get updated a bit but no crazy LTS)
