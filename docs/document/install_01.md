# Quick start

# Preface

Requirements and install mujoco can both follow mujoco-py and mujoco.

# Requirements

cite: [Mujoco-py requirements](https://github.com/openai/mujoco-py#requirements)

The following platforms are currently supported:

 - Linux with Python 3.6+. 

The following platforms are DEPRECATED and unsupported:

Windows support has been DEPRECATED and removed in 2.0.2.0. One known good past version is 1.50.1.68.
Python 2 has been DEPRECATED and removed in 1.50.1.0. Python 2 users can stay on the 0.5 branch. 

# Install MuJoCo

cite: [Mujoco-py install mujoco](https://github.com/openai/mujoco-py#install-mujoco)

1. Obtain a 30-day free trial on the [MuJoCo website](mujoco.org) or free license if you are a student. The license key will arrive in an email with your username and password.

2. Download the MuJoCo version 2.0 binaries for Linux or OSX.

3. Unzip the downloaded `mujoco200` directory into `~/.mujoco/mujoco200`, and place your license key (the `mjkey.txt` file from your email) at `~/.mujoco/mjkey.txt`. If you want to specify a nonstandard location for the key and package, use the env variables `MUJOCO_PY_MJKEY_PATH` and `MUJOCO_PY_MUJOCO_PATH`.

# Install and use mujoco-webpy

1. To install mujoco-webpy, simply use command like:

    ```shell
    git clone https://github.com/yaotc/mujoco-webpy

    pip install ./mujoco-webpy
    ```

2. To render on the web server, all you need to do is change **one line** code. use **`viewer.webrender`** instead of `viewer.render`.

    ```python
    import mujoco_webpy
    from mujoco_webpy.mjviewer import MjWebViewerBasic

    model = mujoco_webpy.load_model_from_xml(MODEL_XML1)
    sim = mujoco_webpy.MjSim(model)

    viewer = MjWebViewerBasic(sim)

    sim.reset()
    for i in range(100):
        sim.step()
        ##################
        #   code here.   #
        ##################
        viewer.webrender()  # use webrender() instead of render().
    ```