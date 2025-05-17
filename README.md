# Hardware h264 encoding using rkmpp

This plugin tries to enable hardware accelerated transcoding profiles using rkmpp on linux. It should be considered experimental and tinkering will certainly be necessary to make this plugin work on your hardware.


For more information on rkmpp and hardware acceleration:

- https://wiki.archlinux.org/index.php/Hardware_video_acceleration#Comparison_tables
- https://github.com/nyanmisaka/ffmpeg-rockchip/wiki


# Building a compatible docker image

Official docker images do not ship with required libraries for hardware transcode.
You can build your own image with the following Dockerfile.rockchip from repo `bro2020/PeerTube`:

[https://github.com/bro2020/PeerTube](https://github.com/bro2020/PeerTube/blob/develop/support/docker/production/Dockerfile.rockchip)

# Running the docker image

In order to access the GPU inside docker, the `docker-compose.yml` should be adapted as follow.
Note that you must find the id of the `render` group on your machine.
You can use `grep render /etc/group | cut -d':' -f3`  to find the id.


```yaml
services:
  peertube-rockchip:
    # or replace image key with your own
    image: bro2020/peertube-rockchip
    # usual peertube configuration
    # ...

    # add these keys
    group_add:
      - <replace with the id of the render group>
    devices:
      # Rkmpp Devices
      - /dev/dri:/dev/dri
      - /dev/dma_heap:/dev/dma_heap
      - /dev/rga:/dev/rga
      - /dev/mpp_service:/dev/mpp_service
```
