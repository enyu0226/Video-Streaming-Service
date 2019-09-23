##A Video Sreaming Service

This is a simple example of a video streaming service implemented using RTP and RSTP adopted from an universiy project.

RTSP is a network control protocol designed for use in entertainment and communications system to control streaming media servers. It is used for establishing and controlling media sessions between end points.

The video in this case is a series of concatenated JPEG-encoded images, with each image being preceded by a 5-byte header which indicates the bit size of the image.

The RTSP client would generate a live video streaming RTP request to the server by sending either SETUP, PLAY, PAUSE and TEARDOWN request with appropriate RTP header. The RTSP server would track the client state and parse bitstreams of the video format to extract JPEG images (conversion into video frame) and encapsulate it within RTP packets and periodically transmit them across the network to reach the corresponding RTSP client. Upon receiving the appropriate RTP packet, The RTSP client would then proceed to de-packetize and display the transmitted video.

To run the client and server on your local machine:

    Open a terminal:
        python Server.py 1025

    Open another terminal:
        python ClientLauncher.py 127.0.0.1 1025 5008 video.mjpeg or
    	python ClientLauncher.py server_host server_port RTP_port video_file

    Where
    	# server_host : the name of the host running the service (ex: localhost)
    	# server_port : port the server is listening (Standard RTSP port is 554, But need to choose a port > 1024)
    	# RTP_port : port where the RTP packets are received ("5008")
    	# video_file : name of video file you want to request,here "video.mjpeg"

Production Design Consideration:

For large-scaled and production on-demand or live streaming service, it is better to use DASH or LHLS as they operate on HTTP (Layer 7) which sits on top of TCP (Layer 4). This is because most enterprise or third party NAT firewall would block UDP (RTP sits on top of UTP) instead of TCP traffic, thereby limiting the streaming of RTP traffic across the network despite the faster speed and performance benefit of UDP compared to TCP in terms of low-latency message delivery that need not be highly reliable. The placement of global network of CDN cluster represents an important method for lowering the latency of on-demand and live streaming video delivery to end-users that operate on the slower (compared to protocols the run on UDP) but reliable HTTP protocol.
