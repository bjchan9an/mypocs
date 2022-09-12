To trigger this bug, use:
```sh
./avconv -y -i ./poc -f null -
```

GDB trace:

```sh
#0  __GI___libc_free (mem=0xcfcfcfcfcfcf) at malloc.c:3113
#1  0x0000000001145a9e in buffer_pool_free (pool=0x0) at /home/realworld/libav-12.3/libavutil/buffer.c:243
#2  0x0000000001145eb5 in pool_release_buffer (opaque=0x1c77460, data=<optimized out>) at /home/realworld/libav-12.3/libavutil/buffer.c:278
#3  0x0000000001145030 in av_buffer_unref (buf=<optimized out>) at /home/realworld/libav-12.3/libavutil/buffer.c:116
#4  0x000000000114fa88 in av_frame_unref (frame=0x1c75c80) at /home/realworld/libav-12.3/libavutil/frame.c:309
#5  0x000000000114f879 in av_frame_free (frame=0x7fffffffdb90) at /home/realworld/libav-12.3/libavutil/frame.c:85
#6  0x0000000000d4a861 in wrapped_avframe_release_buffer (unused=<optimized out>, data=0xcfcfcfcfcfcf <error: Cannot access memory at address 0xcfcfcfcfcfcf>)
    at /home/realworld/libav-12.3/libavcodec/wrapped_avframe.c:39
#7  0x0000000001145030 in av_buffer_unref (buf=<optimized out>) at /home/realworld/libav-12.3/libavutil/buffer.c:116
#8  0x00000000006955cb in av_packet_unref (pkt=0x7fffffffdc00) at /home/realworld/libav-12.3/libavcodec/avpacket.c:350
#9  0x0000000000420ba7 in avconv_cleanup (ret=<optimized out>) at /home/realworld/libav-12.3/avconv.c:211
#10 0x0000000000403d47 in exit_program (ret=0) at /home/realworld/libav-12.3/cmdutils.c:97
#11 0x0000000000420438 in main (argc=<optimized out>, argv=0x7fffffffde48) at /home/realworld/libav-12.3/avconv.c:2913
```