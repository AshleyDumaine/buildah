# buildah-bud "1" "April 2017" "buildah"

## NAME
buildah bud - Build an image using instructions from Dockerfiles.

## SYNOPSIS
**buildah** **bud | build-using-dockerfile** [*options* [...]] [**context**]

## DESCRIPTION
Builds an image using instructions from one or more Dockerfiles and a specified
build context directory.  The build context directory can be specified as the
**http** or **https** URL of an archive which will be retrieved and extracted
to a temporary location.

## OPTIONS

**--add-host**=[]

Add a custom host-to-IP mapping (host:ip)

Add a line to /etc/hosts. The format is hostname:ip. The **--add-host** option can be set multiple times.

**--authfile** *path*

Path of the authentication file. Default is ${XDG_RUNTIME\_DIR}/containers/auth.json, which is set using `podman login`.
If the authorization state is not found there, $HOME/.docker/config.json is checked, which is set using `docker login`.

**--build-arg** *arg=value*

Specifies a build argument and its value, which will be interpolated in
instructions read from the Dockerfiles in the same way that environment
variables are, but which will not be added to environment variable list in the
resulting image's configuration.

**--cert-dir** *path*

Use certificates at *path* (*.crt, *.cert, *.key) to connect to the registry.
Default certificates directory is _/etc/containers/certs.d_.

**--cgroup-parent**=""

Path to cgroups under which the cgroup for the container will be created. If the path is not absolute, the path is considered to be relative to the cgroups path of the init process. Cgroups will be created if they do not already exist.

**--cpu-period**=*0*

Limit the CPU CFS (Completely Fair Scheduler) period

Limit the container's CPU usage. This flag tell the kernel to restrict the container's CPU usage to the period you specify.

**--cpu-quota**=*0*

Limit the CPU CFS (Completely Fair Scheduler) quota

Limit the container's CPU usage. By default, containers run with the full
CPU resource. This flag tell the kernel to restrict the container's CPU usage
to the quota you specify.

**--cpu-shares**=*0*

CPU shares (relative weight)

By default, all containers get the same proportion of CPU cycles. This proportion
can be modified by changing the container's CPU share weighting relative
to the weighting of all other running containers.

To modify the proportion from the default of 1024, use the **--cpu-shares**
flag to set the weighting to 2 or higher.

The proportion will only apply when CPU-intensive processes are running.
When tasks in one container are idle, other containers can use the
left-over CPU time. The actual amount of CPU time will vary depending on
the number of containers running on the system.

For example, consider three containers, one has a cpu-share of 1024 and
two others have a cpu-share setting of 512. When processes in all three
containers attempt to use 100% of CPU, the first container would receive
50% of the total CPU time. If you add a fourth container with a cpu-share
of 1024, the first container only gets 33% of the CPU. The remaining containers
receive 16.5%, 16.5% and 33% of the CPU.

On a multi-core system, the shares of CPU time are distributed over all CPU
cores. Even if a container is limited to less than 100% of CPU time, it can
use 100% of each individual CPU core.

For example, consider a system with more than three cores. If you start one
container **{C0}** with **-c=512** running one process, and another container
**{C1}** with **-c=1024** running two processes, this can result in the following
division of CPU shares:

    PID    container	CPU	CPU share
    100    {C0}		0	100% of CPU0
    101    {C1}		1	100% of CPU1
    102    {C1}		2	100% of CPU2

**--cpuset-cpus**=""

  CPUs in which to allow execution (0-3, 0,1)

**--cpuset-mems**=""

Memory nodes (MEMs) in which to allow execution (0-3, 0,1). Only effective on NUMA systems.

If you have four memory nodes on your system (0-3), use `--cpuset-mems=0,1`
then processes in your container will only use memory from the first
two memory nodes.

**--creds** *creds*

The [username[:password]] to use to authenticate with the registry if required.
If one or both values are not supplied, a command line prompt will appear and the
value can be entered.  The password is entered without echo.

**-f, --file** *Dockerfile*

Specifies a Dockerfile which contains instructions for building the image,
either a local file or an **http** or **https** URL.  If more than one
Dockerfile is specified, *FROM* instructions will only be accepted from the
first specified file.

If a build context is not specified, and at least one Dockerfile is a
local file, the directory in which it resides will be used as the build
context.

**--format**

Control the format for the built image's manifest and configuration data.
Recognized formats include *oci* (OCI image-spec v1.0, the default) and
*docker* (version 2, using schema format 2 for the manifest).

**-m**, **--memory**=""

Memory limit (format: <number>[<unit>], where unit = b, k, m or g)

Allows you to constrain the memory available to a container. If the host
supports swap memory, then the **-m** memory setting can be larger than physical
RAM. If a limit of 0 is specified (not using **-m**), the container's memory is
not limited. The actual limit may be rounded up to a multiple of the operating
system's page size (the value would be very large, that's millions of trillions).

**--memory-swap**="LIMIT"

A limit value equal to memory plus swap. Must be used with the  **-m**
(**--memory**) flag. The swap `LIMIT` should always be larger than **-m**
(**--memory**) value.  By default, the swap `LIMIT` will be set to double
the value of --memory.

The format of `LIMIT` is `<number>[<unit>]`. Unit can be `b` (bytes),
`k` (kilobytes), `m` (megabytes), or `g` (gigabytes). If you don't specify a
unit, `b` is used. Set LIMIT to `-1` to enable unlimited swap.

**--pull**

Pull the image if it is not present.  If this flag is disabled (with
*--pull=false*) and the image is not present, the image will not be pulled.
Defaults to *true*.

**--pull-always**

Pull the image even if a version of the image is already present.

**-q, --quiet**

Suppress output messages which indicate which instruction is being processed,
and of progress when pulling images from a registry, and when writing the
output image.

**--runtime** *path*

The *path* to an alternate OCI-compatible runtime, which will be used to run
commands specified by the **RUN** instruction.

**--runtime-flag** *flag*

Adds global flags for the container rutime. To list the supported flags, please
consult the manpages of the selected container runtime (`runc` is the default
runtime, the manpage to consult is `runc(8)`).
Note: Do not pass the leading `--` to the flag. To pass the runc flag `--log-format json`
to buildah bud, the option given would be `--runtime-flag log-format=json`.

**--security-opt**=[]

Security Options

  "label=user:USER"   : Set the label user for the container
  "label=role:ROLE"   : Set the label role for the container
  "label=type:TYPE"   : Set the label type for the container
  "label=level:LEVEL" : Set the label level for the container
  "label=disable"     : Turn off label confinement for the container
  "no-new-privileges" : Not supported

  "seccomp=unconfined" : Turn off seccomp confinement for the container
  "seccomp=profile.json :  White listed syscalls seccomp Json file to be used as a seccomp filter

  "apparmor=unconfined" : Turn off apparmor confinement for the container
  "apparmor=your-profile" : Set the apparmor confinement profile for the container

**--shm-size**=""

Size of `/dev/shm`. The format is `<number><unit>`. `number` must be greater than `0`.
Unit is optional and can be `b` (bytes), `k` (kilobytes), `m`(megabytes), or `g` (gigabytes).
If you omit the unit, the system uses bytes. If you omit the size entirely, the system uses `64m`.

**--signature-policy** *signaturepolicy*

Pathname of a signature policy file to use.  It is not recommended that this
option be used, as the default behavior of using the system-wide default policy
(frequently */etc/containers/policy.json*) is most often preferred.

**-t, --tag** *imageName*

Specifies the name which will be assigned to the resulting image if the build
process completes successfully.

**--tls-verify** *bool-value*

Require HTTPS and verify certificates when talking to container registries (defaults to true).

**--ulimit**=[]

Ulimit options

**-v**|**--volume**[=*[HOST-DIR:CONTAINER-DIR[:OPTIONS]]*]

   Create a bind mount. If you specify, ` -v /HOST-DIR:/CONTAINER-DIR`, podman
   bind mounts `/HOST-DIR` in the host to `/CONTAINER-DIR` in the podman
   container. The `OPTIONS` are a comma delimited list and can be:

   * [rw|ro]
   * [z|Z]
   * [`[r]shared`|`[r]slave`|`[r]private`]

The `CONTAINER-DIR` must be an absolute path such as `/src/docs`. The `HOST-DIR`
must be an absolute path as well. podman bind-mounts the `HOST-DIR` to the
path you specify. For example, if you supply the `/foo` value, podman creates a bind-mount.

You can specify multiple  **-v** options to mount one or more mounts to a
container.

You can add `:ro` or `:rw` suffix to a volume to mount it read-only or
read-write mode, respectively. By default, the volumes are mounted read-write.
See examples.

Labeling systems like SELinux require that proper labels are placed on volume
content mounted into a container. Without a label, the security system might
prevent the processes running inside the container from using the content. By
default, podman does not change the labels set by the OS.

To change a label in the container context, you can add either of two suffixes
`:z` or `:Z` to the volume mount. These suffixes tell podman to relabel file
objects on the shared volumes. The `z` option tells podman that two containers
share the volume content. As a result, podman labels the content with a shared
content label. Shared volume labels allow all containers to read/write content.
The `Z` option tells podman to label the content with a private unshared label.
Only the current container can use a private volume.

By default bind mounted volumes are `private`. That means any mounts done
inside container will not be visible on the host and vice versa. This behavior can
be changed by specifying a volume mount propagation property. 

When the mount propagation policy is set to `shared`, any mounts completed inside
the container on that volume will be visible to both the host and container. When
the mount propagation policy is set to `slave`, one way mount propagation is enabled
and any mounts completed on the host for that volume will be visible only inside of the container.
To control the mount propagation property of volume use the `:[r]shared`,
`:[r]slave` or `:[r]private` propagation flag. The propagation property can
be specified only for bind mounted volumes and not for internal volumes or
named volumes. For mount propagation to work on the source mount point (mount point
where source dir is mounted on) has to have the right propagation properties. For
shared volumes, the source mount point has to be shared. And for slave volumes,
the source mount has to be either shared or slave.

Use `df <source-dir>` to determine the source mount and then use
`findmnt -o TARGET,PROPAGATION <source-mount-dir>` to determine propagation
properties of source mount, if `findmnt` utility is not available, the source mount point
can be determined by looking at the mount entry in `/proc/self/mountinfo`. Look
at `optional fields` and see if any propagaion properties are specified.
`shared:X` means the mount is `shared`, `master:X` means the mount is `slave` and if
nothing is there that means the mount is `private`.

To change propagation properties of a mount point use the `mount` command. For
example, to bind mount the source directory `/foo` do
`mount --bind /foo /foo` and `mount --make-private --make-shared /foo`. This
will convert /foo into a `shared` mount point.  The propagation properties of the source
mount can be changed directly. For instance if `/` is the source mount for
`/foo`, then use `mount --make-shared /` to convert `/` into a `shared` mount.

## EXAMPLE

buildah bud .

buildah bud -f Dockerfile.simple .

buildah bud -f Dockerfile.simple -f Dockerfile.notsosimple

buildah bud -t imageName .

buildah bud --tls-verify=true -t imageName -f Dockerfile.simple

buildah bud --tls-verify=false -t imageName .

buildah bud --runtime-flag log-format=json .

buildah bud --runtime-flag debug .

buildah bud --authfile /tmp/auths/myauths.json --cert-dir ~/auth --tls-verify=true --creds=username:password -t imageName -f Dockerfile.simple

buildah bud --memory 40m --cpu-period 10000 --cpu-quota 50000 --ulimit nofile=1024:1028 -t imageName .

buildah bud --security-opt label=level:s0:c100,c200 --cgroup-parent /path/to/cgroup/parent -t imageName .

buildah bud --volume /home/test:/myvol:ro,Z -t imageName .

## SEE ALSO
buildah(1), podman-login(1), docker-login(1)
