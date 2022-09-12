To trigger this bug, use:

```sh
./avconv -y -i ./poc -f null -
```

GDB Trace
```sh
[Switching to Thread 0x7ffff7207700 (LWP 18009)]
0x000000000102ad89 in ff_put_pixels8_y2_mmxext.loop () at /home/realworld/libav-12.3/libavcodec/x86/hpeldsp.asm:174
174     PUT_PIXELS8_Y2
(gdb) bt
#0  0x000000000102ad89 in ff_put_pixels8_y2_mmxext.loop () at /home/realworld/libav-12.3/libavcodec/x86/hpeldsp.asm:174
#1  0x0000000000d5107c in put_pixels16_y2_mmxext (block=0x7fffce790040 "", pixels=0x1d4fe40 "", line_size=23360, h=16) at /home/realworld/libav-12.3/libavcodec/x86/hpeldsp_init.c:149
#2  0x0000000000a18cf1 in mpeg_motion_internal (s=<optimized out>, dest_y=<optimized out>, dest_cb=<optimized out>, dest_cr=<optimized out>, field_based=0, bottom_field=0, 
    field_select=<optimized out>, ref_picture=<optimized out>, pix_op=<optimized out>, motion_x=<optimized out>, motion_y=<optimized out>, h=<optimized out>, is_mpeg12=1, 
    mb_y=<optimized out>) at /home/realworld/libav-12.3/libavcodec/mpegvideo_motion.c:361
#3  mpeg_motion (s=<optimized out>, dest_y=<optimized out>, dest_cb=<optimized out>, dest_cr=<optimized out>, field_select=<optimized out>, ref_picture=<optimized out>, pix_op=0x1c7f828, 
    motion_x=0, motion_y=7, h=16, mb_y=0) at /home/realworld/libav-12.3/libavcodec/mpegvideo_motion.c:383
#4  0x0000000000a17526 in mpv_motion_internal (s=<optimized out>, dest_y=<optimized out>, dest_cb=<optimized out>, dest_cr=<optimized out>, ref_picture=<optimized out>, 
    pix_op=<optimized out>, is_mpeg12=1, dir=<optimized out>, qpix_op=<optimized out>) at /home/realworld/libav-12.3/libavcodec/mpegvideo_motion.c:891
#5  ff_mpv_motion (s=<optimized out>, dest_y=<optimized out>, dest_cb=<optimized out>, dest_cr=<optimized out>, dir=<optimized out>, ref_picture=<optimized out>, pix_op=<optimized out>, 
    qpix_op=<optimized out>) at /home/realworld/libav-12.3/libavcodec/mpegvideo_motion.c:962
#6  0x00000000009e5eee in mpv_decode_mb_internal (s=<optimized out>, block=<optimized out>, is_mpeg12=1) at /home/realworld/libav-12.3/libavcodec/mpegvideo.c:1599
#7  ff_mpv_decode_mb (s=0x1c7eea0, block=<optimized out>) at /home/realworld/libav-12.3/libavcodec/mpegvideo.c:1731
#8  0x000000000096b83d in mpeg_decode_slice (s=0x1c7eea0, mb_y=<optimized out>, buf=<optimized out>, buf_size=<optimized out>) at /home/realworld/libav-12.3/libavcodec/mpeg12dec.c:1832
#9  0x0000000000969e81 in slice_decode_thread (c=0x1d0a5e0, arg=<optimized out>) at /home/realworld/libav-12.3/libavcodec/mpeg12dec.c:1945
#10 0x0000000000fd49d3 in worker (v=0x1d0a5e0) at /home/realworld/libav-12.3/libavcodec/pthread_slice.c:91
#11 0x00007ffff76006db in start_thread (arg=0x7ffff7207700) at pthread_create.c:463
#12 0x00007ffff732961f in clone () at ../sysdeps/unix/sysv/linux/x86_64/clone.S:95
```