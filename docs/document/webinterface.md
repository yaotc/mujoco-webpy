# Web render Interface



# Web Render Process

> **web render engine** is independent of any Physics Engine. That is to say, every engine can render its object so long as it's in accord with [**Interface Standard**](document/webinterface?id=interface-standard).


As **Physics engine**:

>    step 1. initial object and camera parameters.
> 
>    step 2. *Physics engine* generate `Basic File` to `PATH_TO_NODE`.
> 
>    step 3. *web render engine* pull `Basic File` from `PATH_TO_NODE` and start render, then generate `Interaction File` to `PATH_TO_NODE` every `render timestep`.
> 
>    step 4. *Physics engine* pull `Interaction File` from `PATH_TO_NODE` and transform interaction into dynamics variables to finish simulation  every `simulation timestep`, then return to **step 2**. 

!> `render timestep` may be greater than or equal `simulation timestep`, it depends on server resources, network delay, file sizes and so on.

As **web render engine**:

>    step 1. initial render spaces and camera.
> 
>    step 2. *web render engine* pull `Basic File` from `PATH_TO_NODE` and start render, then generate `Interaction File` to `PATH_TO_NODE` every `render timestep`.
>    step 3. *web render engine* will stop work when the process ends. 



# Interface Standard

## Basic Json File Standard

### Parameters

| name            | type   | range    | default            | describe |
|:-------:        | :---:  | :-------:| :------:           |:------- |
| id              |  int   |  [0, -]  |    0               |  id |
| body_name       |  str   |    -     | "world"            |  body_name |
| body_pos        |array[3]|    -     |[0.0, 0.0, 0.0]     |  position offset rel. to parent body |
| body_quat       |array[4]|    -     |[1.0, 0.0, 0.0, 0.0]|  orientation offset rel. to parent body | 
| geom_name       |  str   |    -     |  "null"            |  geom name |
| geom_type       |  int   |  [0, 7]  |    -               |  support geom types, 0: plane; 1: height field; 2: sphere; 3: capsule; 4: ellipsoid; 5: cylinder; 6: box; 7: mesh |
| geom_size       |array[3]|    -     |    -               |  geom-specific size parameters |
| gemo_rgba       |array[4]|    -     |    -               |  rgba when material is omitted | 
| geom_pos        |array[3]|    -     |    -               |   local position offset rel. to body |
| geom_quat       |array[4]|    -     |    -               |  local orientation offset rel. to body |               
| cam_type        |  int   |  [0, 1]  |    0               |  0: free camera; 1: tracking camera, uses trackbodyid |  
| can_trackbodyid |  int   |    -     |    0               |  trackbodyid | 
| cam_lookat      |array[3]|    -     |    -               |  lookat point |
| cam_azimuth     | float  | [0., 90.]|   90.0             |  distance to lookat point or tracked body |
| cam_distance    | float  |    -     |    1.0             |  camera azimuth (deg) |
| cam_elevation   | float  |    -     |  -45.0             |  camera elevation (deg) |
          



### Demo

Basic json file:

```json

"[
{\"id\": 0,
 \"body_name\": \"world\",
 \"body_pos\": [0.0, 0.0, 0.0],
 \"body_quat\": [1.0, 0.0, 0.0, 0.0],
 \"geom_name\": null,
 \"geom_type\": 0,
 \"geom_size\": [1.0, 1.0, 0.1],
 \"gemo_rgba\": [0.5, 0.5, 0.5, 1.0],
 \"geom_pos\": [0.0, 0.0, 0.0],
 \"geom_quat\": [1.0, 0.0, 0.0, 0.0],
 \"cam_type\": 0,
 \"can_trackbodyid\": -1,
 \"cam_lookat\": [0.0, 0.0, 0.05],
 \"cam_azimuth\": 90.0,
 \"cam_distance\": 0.9891287847477921,
 \"cam_elevation\": -45.0}, 

 {\"id\": 1,
 \"body_name\": \"body1\",
 \"body_pos\": [0.22, 0.0, 0.1],
 \"body_quat\": [1.0, 0.0, 0.0, 0.0],
 \"geom_name\": \"sf1\",
 \"geom_type\": 2,
 \"geom_size\": [0.1, 0.0, 0.0],
 \"gemo_rgba\": [0.10000000149011612, 0.800000011920929, 0.800000011920929, 1.0],
 \"geom_pos\": [0.0, 0.0, 0.0],
 \"geom_quat\": [1.0, 0.0, 0.0, 0.0],
 \"cam_type\": 0,
 \"can_trackbodyid\": -1,
 \"cam_lookat\": [0.0, 0.0, 0.05],
 \"cam_azimuth\": 90.0,
 \"cam_distance\": 0.9891287847477921,
 \"cam_elevation\": -45.0}, 

 {\"id\": 2,
 \"body_name\": \"body2\",
 \"body_pos\": [-0.22, 0.0, 0.05],
 \"body_quat\": [1.0, 0.0, 0.0, 0.0],
 \"geom_name\": \"sf2\",
 \"geom_type\": 6,
 \"geom_size\": [0.1, 0.2, 0.05],
 \"gemo_rgba\": [1.0, 1.0, 0.0, 1.0],
 \"geom_pos\": [-0.22, 0.0, 0.0],
 \"geom_quat\": [1.0, 0.0, 0.0, 0.0],
 \"cam_type\": 0,
 \"can_trackbodyid\": -1,
 \"cam_lookat\": [0.0, 0.0, 0.05],
 \"cam_azimuth\": 90.0,
 \"cam_distance\": 0.9891287847477921,
 \"cam_elevation\": -45.0}

]"

```


## Interaction Json File Standard

### Parameters


| name       | type | range | default | describe |
|:-------:   | :-------:| :-------: | :-------:  |:------- |
| flag_pert  | int |  [0, 1] |   1     | if *flag_pert* is 1: if *flag_paused* is 1, callback `move pose`, and if *flag_paused* is 0, callback `move pert`; if *flag_pert* is: None. *flag_pert* and *flag_cam* can't be both 0|
| flag_cam   | int |  [0, 1] |   0     | if 1: callback function `move camera`; if 0: None. *flag_cam* and *flag_pert* can't be both 0|
| flag_paused| int |  [0, 1] |   0     | only work if flag_pert is 1 |
| select_id  | int |  [0, -) |  1     | select body id | 
| move_action| int | [0, 1, 2, 3, 4, 5] | 1 | mouse action:  0: no action; 1: rotate, vertical plane; 2: rotate, horizontal plane; 3: move, vertical plane; 4: move, horizontal plane; 5: zoom |
| dx         | float| (-1, 1) |    0.0  |   relative dx |
| dy         | float| (-1, 1) |    0.0  |   relative dy |
| track_flag | int  | [0, 1] | 0 | if 1: camera will track num *track_id* object.|
| track_id   | int  | [0, -) |  0   | if *track_flag* is 1, *track_id* is active.|


### Demo

Interaction json file:

```json
"{
 \"flag_pert\": 0,
 \"flag_cam\": 0,
 \"flag_paused\": 0,
 \"select_id\": 0,
 \"move_action\": 3,
 \"dx\": 0.0,
 \"dy\": 0.0,
 \"track_flag\": 0,
 \"track_id\": 0
}"
```