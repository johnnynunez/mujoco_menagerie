# Seeed Studio reBot DevArm (RobStride) Description (MJCF)

> [!IMPORTANT]
> Requires MuJoCo 2.3.3 or later.

## Changelog

See [CHANGELOG.md](./CHANGELOG.md) for a full history of changes.

## Overview

This package contains a robot description (MJCF) of the **reBot DevArm**, a
6-DOF manipulator with a 2-finger parallel gripper built by
[Seeed Studio](https://www.seeedstudio.com/) around
[RobStride](https://robstride.com/) quasi-direct-drive actuators (RS-06 for the
shoulder/elbow, RS-00 for the wrist and gripper). It is derived from the
publicly available URDF in the
[Seeed-Projects/reBot-Isaacsim](https://github.com/Seeed-Projects/reBot-Isaacsim)
repository.

<p float="left">
  <img src="seeed_rebot_devarm.png" width="400">
</p>

### URDF → MJCF derivation steps

1. Converted the URDF body tree with [`urdf-to-mjcf`](https://github.com/discoverse-dev/urdf-to-mjcf),
   using CoACD to generate convex collision meshes.
2. Kept `gripper_end` as a separate welded body and preserved all six components
   of every URDF inertia tensor, including products of inertia (MuJoCo's built-in
   URDF importer mis-rotates the merged
   fixed-joint inertial, which introduces a spurious ~1 N·m gravity torque at
   the wrist).
3. Fixed the base to the world and added the base link inertial.
4. Extracted joint and actuator properties into `<default>` classes.
5. Added position-controlled actuators. Joint torque limits come from the
   actuator ratings (RS-06: 36 N·m, RS-00: 14 N·m).
6. Added `home` and `raised` keyframes.
7. Recovered the per-part colours (lime accent covers, black motors and
   gripper, aluminium brackets) from the mesh filenames — the source URDF
   stores every visual as flat grey.
8. Added `scene.xml` which includes the robot, a textured ground plane, skybox
   and haze.

### Validation

Mass, center of mass, and the full compiled inertia tensor were verified against
the source URDF for all 10 bodies. The generalized gravity torque g(q) was also
verified against Pinocchio (built from the same URDF) across several poses; the
two agree to under 1e-5 N·m. Pinocchio in turn matches the NVIDIA Isaac Sim
(PhysX and Newton) drive droop and the on-hardware measurements, so the model is
dynamically consistent with the URDF, the vendor's Isaac Sim asset, and the
physical arm.

## License

This model is released under an [Apache-2.0 License](LICENSE).
