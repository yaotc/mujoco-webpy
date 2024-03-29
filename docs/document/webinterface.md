# Web Render Interface



# Web Render Process

> **web render engine** is independent of any Physics Engine. That is to say, every engine can render its object so long as it's in accord with [**Interface Standard**](document/webinterface?id=interface-standard).


As **Physics engine**:

>    step 1. initial object and camera parameters.
> 
>    step 2. *Physics engine* generate `Basic File` and push to `PATH_TO_NODE`.
> 
>    step 3. *Physics engine* pull `Interaction File` from `PATH_TO_NODE`, and transform interaction into dynamics variables to finish simulation every `time 1`, then return to **step 2**. 


As **web render engine**:

>    step 1. initial render spaces and camera.
> 
>    step 2. *web render engine* pull `Basic File` from `PATH_TO_NODE`, and transform basic object parameters into proper variables to finish render every `time 3`, then generate `Interaction File` and push to `PATH_TO_NODE` every `time 2`.
> 
>    step 3. *web render engine* will stop work when the process ends. 

![webrender](images/webrender6.png)

!> **Notice**: $$ time 1 \le time 2 \le time 3 $$ It depends on server resources, network delay, file sizes and so on.


# Interface Standard

## Basic Json File Standard

### Parameters

| name            | type   | range    | default            | describe |
|:-------:        | :---:  | :-------:| :------:           |:------- |
| id              |  int   |  [0, -]  |    0               |  id |
| body_name       |  str   |    -     | "world"            |  body_name |
| body_pos        |$$ array[3] $$|    -     |[0.0, 0.0, 0.0]     |  Cartesian position of body frame |
| body_quat       |$$ array[4] $$|    -     |[1.0, 0.0, 0.0, 0.0]|  Cartesian orientation of body frame | 
| body_xmat       |$$ array[9] $$|    -     |-|  Cartesian orientation of body frame | 
| geom_name       |  str   |    -     |  "null"            |  geom name |
| geom_type       |  int   |  [0, 7]  |    -               |  support geom types, 0: plane; 1: height field; 2: sphere; 3: capsule; 4: ellipsoid; 5: cylinder; 6: box; 7: mesh |
| geom_size       |$$ array[3] $$|    -     |    -               |  geom-specific size parameters |
| gemo_rgba       |$$ array[4] $$|    -     |    -               |  rgba when material is omitted | 
| geom_pos        |$$ array[3] $$|    -     |    -               |  Cartesian geom position  |
| geom_xmat       |$$ array[9] $$|    -     |    -               |  Cartesian geom orientation | 
| cam_type        |  int   |  [0, 1]  |    0               |  0: free camera; 1: tracking camera, uses trackbodyid |  
| can_trackbodyid |  int   |    -     |    0               |  trackbodyid | 
| cam_lookat      |$$ array[3] $$|    -     |    -               |  lookat point |
| cam_azimuth     | float  | [0., 90.]|   90.0             |  distance to lookat point or tracked body |
| cam_distance    | float  |    -     |    1.0             |  camera azimuth (deg) |
| cam_elevation   | float  |    -     |  -45.0             |  camera elevation (deg) |
| GLCam_pos       |$$ 2\times array[3] $$ | -      |  [[0.0, 0.0, 0.0], [0.0, 0.0, 0.0]]   | position|
| GLCam_forward   |$$ 2\times array[3] $$ | -       |  [[0.0, 0.0, 0.0], [0.0, 0.0, 0.0]]  | forward direction|
| GLCam_up        |$$ 2\times array[3] $$ | -      |  [[0.0, 0.0, 0.0], [0.0, 0.0, 0.0]]   | up direction|
| GLCam_frustum_center | float| -     | -                  | hor. center (left,right set to match aspect)|
| GLCam_frustum_bottom | float| -     | -                  | bottom|
| GLCam_frustum_top    | float| -     | -                  | top|
| GLCam_frustum_near   | float| -     | -                  | near|
| GLCam_frustum_far    | float| -     | -                  | far|

### Demo

Basic json file:

```json

"[
{\"id\": 0, 
\"body_name\": \"world\", 
\"body_pos\": [0.0, 0.0, 0.0], 
\"body_quat\": [1.0, 0.0, 0.0, 0.0], 
\"body_xmat\": [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0], 
\"geom_name\": null, 
\"geom_type\": 0, 
\"geom_size\": [1.0, 1.0, 0.1], 
\"gemo_rgba\": [0.5, 0.5, 0.5, 1.0], 
\"geom_pos\": [0.0, 0.0, 0.0], 
\"geom_xmat\": [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0], 
\"cam_type\": 1, 
\"can_trackbodyid\": 2, 
\"cam_lookat\": [-0.43998083529709503, 4.955485360570072e-20, 0.049890396752983665], 
\"cam_azimuth\": 90.0, 
\"cam_distance\": 0.9891287847477921, 
\"cam_elevation\": -45.0, 
\"GLCam_pos\": [[-0.4739808440208435, -0.6994196772575378, 0.7493100762367249], [-0.40598082542419434, -0.6994196772575378, 0.7493100762367249]], 
\"GLCam_forward\": [[4.329780301713277e-17, 0.7071067690849304, -0.7071067690849304], [4.329780301713277e-17, 0.7071067690849304, -0.7071067690849304]], 
\"GLCam_up\": [[4.329780301713277e-17, 0.7071067690849304, 0.7071067690849304], [4.329780301713277e-17, 0.7071067690849304, 0.7071067690849304]], 
\"GLCam_frustum_center\": [0.0, 0.0], 
\"GLCam_frustum_bottom\": [-0.004097105469554663, -0.004097105469554663], 
\"GLCam_frustum_top\": [0.004097105469554663, 0.004097105469554663], 
\"GLCam_frustum_near\": [0.009891287423670292, 0.009891287423670292], 
\"GLCam_frustum_far\": [49.45643997192383, 49.45643997192383]}, 

{\"id\": 1, 
\"body_name\": \"body1\", 
\"body_pos\": [0.22, 4.706245490306578e-20, 0.0996328160381056], 
\"body_quat\": [1.0, 0.0, 0.0, 0.0], 
\"body_xmat\": [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0], 
\"geom_name\": \"sf1\", 
\"geom_type\": 2, 
\"geom_size\": [0.1, 0.0, 0.0], 
\"gemo_rgba\": [0.10000000149011612, 0.800000011920929, 0.800000011920929, 1.0], 
\"geom_pos\": [0.22, 4.706245490306578e-20, 0.0996328160381056], 
\"geom_xmat\": [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0], 
\"cam_type\": 1, 
\"can_trackbodyid\": 2, 
\"cam_lookat\": [-0.43998083529709503, 4.955485360570072e-20, 0.049890396752983665], 
\"cam_azimuth\": 90.0, 
\"cam_distance\": 0.9891287847477921, 
\"cam_elevation\": -45.0, 
\"GLCam_pos\": [[-0.4739808440208435, -0.6994196772575378, 0.7493100762367249], [-0.40598082542419434, -0.6994196772575378, 0.7493100762367249]], 
\"GLCam_forward\": [[4.329780301713277e-17, 0.7071067690849304, -0.7071067690849304], [4.329780301713277e-17, 0.7071067690849304, -0.7071067690849304]], 
\"GLCam_up\": [[4.329780301713277e-17, 0.7071067690849304, 0.7071067690849304], [4.329780301713277e-17, 0.7071067690849304, 0.7071067690849304]], 
\"GLCam_frustum_center\": [0.0, 0.0], 
\"GLCam_frustum_bottom\": [-0.004097105469554663, -0.004097105469554663], 
\"GLCam_frustum_top\": [0.004097105469554663, 0.004097105469554663], 
\"GLCam_frustum_near\": [0.009891287423670292, 0.009891287423670292], 
\"GLCam_frustum_far\": [49.45643997192383, 49.45643997192383]}, 

{\"id\": 2, 
\"body_name\": \"body2\", 
\"body_pos\": [-0.22, 4.863814203205803e-20, 0.04989161695445766], 
\"body_quat\": [1.0, 0.0, 0.0, 0.0], 
\"body_xmat\": [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0], 
\"geom_name\": \"sf2\", 
\"geom_type\": 6, 
\"geom_size\": [0.1, 0.2, 0.05], 
\"gemo_rgba\": [1.0, 1.0, 0.0, 1.0], 
\"geom_pos\": [-0.44, 4.863814203205803e-20, 0.04989161695445766], 
\"geom_xmat\": [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0], 
\"cam_type\": 1, 
\"can_trackbodyid\": 2, 
\"cam_lookat\": [-0.43998083529709503, 4.955485360570072e-20, 0.049890396752983665], 
\"cam_azimuth\": 90.0, 
\"cam_distance\": 0.9891287847477921, 
\"cam_elevation\": -45.0, 
\"GLCam_pos\": [[-0.4739808440208435, -0.6994196772575378, 0.7493100762367249], [-0.40598082542419434, -0.6994196772575378, 0.7493100762367249]], 
\"GLCam_forward\": [[4.329780301713277e-17, 0.7071067690849304, -0.7071067690849304], [4.329780301713277e-17, 0.7071067690849304, -0.7071067690849304]], 
\"GLCam_up\": [[4.329780301713277e-17, 0.7071067690849304, 0.7071067690849304], [4.329780301713277e-17, 0.7071067690849304, 0.7071067690849304]], 
\"GLCam_frustum_center\": [0.0, 0.0], 
\"GLCam_frustum_bottom\": [-0.004097105469554663, -0.004097105469554663], 
\"GLCam_frustum_top\": [0.004097105469554663, 0.004097105469554663], 
\"GLCam_frustum_near\": [0.009891287423670292, 0.009891287423670292], 
\"GLCam_frustum_far\": [49.45643997192383, 49.45643997192383]}
]"

```


## Interaction Json File Standard

### Parameters


| name       | type | range | default | describe |
|:-------:   | :-------:| :-------: | :-------:  |:------- |
| flag_pert  | int |  [0, 1] |   1     | if *flag_pert* is 1: if *flag_paused* is 1, callback `move pose`, and if *flag_paused* is 0, callback `move pert`; if *flag_pert* is 0: None. *flag_pert* and *flag_cam* can't be both 1|
| flag_cam   | int |  [0, 1] |   0     | if 1: callback function `move camera`; if 0: None. *flag_cam* and *flag_pert* can't be both 1|
| flag_paused| int |  [0, 1] |   0     | only work if flag_pert is 1 |
| select_id  | int |  [0, -) |  1     | select body id | 
| move_action| int | [0, 1, 2, 3, 4, 5] | 1 | 0: no action; 1: rotate, vertical plane; 2: rotate, horizontal plane; 3: move, vertical plane; 4: move, horizontal plane; 5: zoom |
| dx         | float| (-1, 1) |    0.0  |   relative dx |
| dy         | float| (-1, 1) |    0.0  |   relative dy |
| track_flag | int  | [0, 1] | 0 | if 1: camera will track num *track_id* object|
| track_id   | int  | [0, -) |  0   | if *track_flag* is 1, *track_id* is active|


### Demo

Interaction json file:

```json

"{\"flag_pert\": 0,
 \"flag_cam\": 1,
 \"flag_paused\": 0,
 \"select_id\": 0,
 \"move_action\": 3,
 \"dx\": 0.0,
 \"dy\": -0.005,
 \"track_flag\": 1,
 \"track_id\": 2
}"

```