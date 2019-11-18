# Functions


## `set_physices_para(name="timestep", value=0.005)` [[source]](http://www.mujoco.org/book/XMLreference.html)

to set physices parameters, can appear many time


**Parameters:**	   
- name list:
    - Physics para: 'integrator','collision','cone','jacobian','solver'

    - Algorithmic para: 'timestep','iterations','tolerance','noslip_iterations',
   'noslip_tolerance','mpr_iterations','mpr_tolerance','apirate'

    - Physical para: 'gravity','wind','magnetic','density','viscosity','impratio'

    - Contact Override: 'o_margin','o_solimp','o_solref'
                
**Returns:**	

- None.

**Raises:**

- **ValueError** – If the value is invalid or out of range.


**parameters describe**

> cite: [MuJoCo XMLreference](http://www.mujoco.org/book/XMLreference.html) 

    
   - timestep : real, "0.002"

    Simulation time step in seconds. This is the single most important parameter affecting the speed-accuracy trade-off which is inherent in every physics simulation. Smaller values result in better accuracy and stability. To achieve real-time performance, the time step must be larger than the CPU time per step (or 4 times larger when using the RK4 integrator). The CPU time is measured with internal timers. It should be monitored when adjusting the time step. MuJoCo can simulate most robotic systems a lot faster than real-time, however models with many floating objects (resulting in many contacts) are more demanding computationally. Keep in mind that stability is determined not only by the time step but also by the Solver parameters; in particular softer constraints can be simulated with larger time steps. When fine-tuning a challenging model, it is recommended to experiment with both settings jointly. In optimization-related applications, real-time is no longer good enough and instead it is desirable to run the simulation as fast as possible. In that case the time step should be made as large as possible.
    
    
   - apirate : real, "100"

    This parameter determines the rate (in Hz) at which the socket API in HAPTIX allows the update function to be executed. This mechanism is used to simulate devices with limited communication bandwidth. It only affects the socket API and not the physics simulation.


   - impratio : real, "1"

    This attribute determines the ratio of frictional-to-normal constraint impedance for elliptic friction cones. The setting of solimp determines a single impedance value for all contact dimensions, which is then modulated by this attribute. Settings larger than 1 cause friction forces to be "harder" than normal forces, having the general effect of preventing slip, without increasing the actual friction coefficient. For pyramidal friction cones the situation is more complex because the pyramidal approximation mixes normal and frictional dimensions within each basis vector; but the overall effect of this attribute is qualitatively similar.


   - gravity : real(3), "0 0 -9.81"

    Gravitational acceleration vector. In the default world orientation the Z-axis points up. The MuJoCo GUI is organized around this convention (both the camera and perturbation commands are based on it) so we do not recommend deviating from it.


   - wind : real(3), "0 0 0"

    Velocity vector of the medium (i.e. wind). This vector is subtracted from the 3D translational velocity of each body, and the result is used to compute viscous, lift and drag forces acting on the body; recall Passive forces in the Computation chapter. The magnitude of these forces scales with the values of the next two attributes.


   - magnetic : real(3), "0 -0.5 0"

    Global magnetic flux. This vector is used by magnetometer sensors, which are defined as sites and return the magnetic flux at the site position expressed in the site frame.


   - density : real, "0"

    Density of the medium, not to be confused with the geom density used to infer masses and inertias. This parameter is used to simulate lift and drag forces, which scale quadratically with velocity. In SI units the density of air is around 1.2 while the density of water is around 1000 depending on temperature. Setting density to 0 disables lift and drag forces.


   - viscosity : real, "0"

    Viscosity of the medium. This parameter is used to simulate viscous forces, which scale linearly with velocity. In SI units the viscosity of air is around 0.00002 while the viscosity of water is around 0.0009 depending on temperature. Setting viscosity to 0 disables viscous forces. Note that the Euler integrator handles damping in the joints implicitly - which improves stability and accuracy. It does not presently do this with body viscosity. Therefore, if the goal is merely to create a damped simulation (as opposed to modeling the specific effects of viscosity), we recommend using joint damping rather than body viscosity. There is a plan to develop an integrator that is fully implicit in velocity, which will make joint damping and body viscosity equally stable, but this feature is not yet available.


   - o_margin : real, "0"

    This attribute replaces the margin parameter of all active contact pairs when Contact override is enabled. Otherwise MuJoCo uses the element-specific margin attribute of geom or pair depending on how the contact pair was generated. See also Collision detection in the Computation chapter. The related gap parameter does not have a global override.


   - o_solref, o_solimp
   
    These attributes replace the solref and solimp parameters of all active contact pairs when contact override is enabled. See Solver parameters for details.


   - integrator : [Euler, RK4], "Euler"

    This attribute selects the numerical integrator to be used. Currently the available integrators are the semi-implicit Euler method and the fixed-step 4-th order Runge Kutta method.


   - collision : [all, predefined, dynamic], "all"

    This attribute specifies which geom pairs should be checked for collision; recall Collision detection in the Computation chapter. "predefined" means that only the explicitly-defined contact pairs are checked. "dynamic" means that only the contact pairs generated dynamically are checked. "all" means that the contact pairs from both sources are checked.


   - cone : [pyramidal, elliptic], "pyramidal"

    The type of contact friction cone. Elliptic cones are a better model of the physical reality, but pyramidal cones sometimes make the solver faster and more robust.


   - jacobian : [dense, sparse, auto], "auto"

    The type of constraint Jacobian and matrices computed from it. Auto resolves to dense when the number of degrees of freedom is up to 60, and sparse over 60.


   - solver : [PGS, CG, Newton], "Newton"

    This attribute selects one of the constraint solver algorithms described in the Computation chapter. Guidelines for solver selection and parameter tuning are available in the Algorithms section above.


   - iterations : int, "100"

    Maximum number of iterations of the constraint solver. When the warmstart attribute of flag is enabled (which is the default), accurate results are obtained with fewer iterations. Larger and more complex systems with many interacting constraints require more iterations. Note that mjData.solver contains statistics about solver convergence, also shown in the profiler.


   - tolerance : real, "1e-8"

    Tolerance threshold used for early termination of the iterative solver. For PGS, the threshold is applied to the cost improvement between two iterations. For CG and Newton, it is applied to the smaller of the cost improvement and the gradient norm. Set the tolerance to 0 to disable early termination.


   - noslip_iterations : int, "0"

    Maximum number of iterations of the Noslip solver. This is a post-processing step executed after the main solver. It uses a modified PGS method to suppress slip/drift in friction dimensions resulting from the soft-constraint model. The default setting 0 disables this post-processing step.


   - noslip_tolerance : real, "1e-6"

    Tolerance threshold used for early termination of the Noslip solver.


   - mpr_iterations : int, "50"

    Maximum number of iterations of the MPR algorithm used for convex mesh collisions. This rarely needs to be adjusted, except in situations where some geoms have very large aspect ratios.


   - mpr_tolerance : real, "1e-6"

    Tolerance threshold used for early termination of the MPR algorithm.