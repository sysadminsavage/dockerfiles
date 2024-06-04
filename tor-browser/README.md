# Usage
## Build the docker image and run it
```
# clone the repo (actually only the Dockerfile is needed... wget or curl would work here too)
git clone 

# build the image
docker build . -t tor-browser

# run it
docker run --log-driver none --rm -v /dev/shm:/dev/shm -v /tmp/.X11-unix:/tmp/.X11-unix:ro -e DISPLAY=$DISPLAY
```

tor-browser should execute in the docker container and display on the local screen

Note on X11:
```
xhost +"local:docker@"
```

Note on Wayland:
sugest x11docker for running X11 apps under wayland.   
```
x11docker tor-browser
```

