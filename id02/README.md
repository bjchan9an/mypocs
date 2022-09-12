To trigger this bug, use:
```sh
./avconv -y -i ./poc -f null -
```

GDB trace:

```sh
#0  malloc_consolidate (av=av@entry=0x7ffff7391b20 <main_arena>) at malloc.c:4176
#1  0x00007ffff704ed0c in _int_malloc (av=av@entry=0x7ffff7391b20 <main_arena>, bytes=bytes@entry=1136) at malloc.c:3457
#2  0x00007ffff704fc35 in _int_memalign (av=av@entry=0x7ffff7391b20 <main_arena>, alignment=alignment@entry=32, bytes=bytes@entry=1056) at malloc.c:4435
#3  0x00007ffff705479d in _mid_memalign (address=<optimized out>, bytes=1056, alignment=32) at malloc.c:3127
#4  __posix_memalign (memptr=memptr@entry=0x7fffffffd0a0, alignment=alignment@entry=32, size=size@entry=1056) at malloc.c:5042
#5  0x0000000000b08c3f in av_malloc (size=1056) at /home/realworld/libav-12.3/libavutil/mem.c:81
#6  av_mallocz (size=size@entry=1056) at /home/realworld/libav-12.3/libavutil/mem.c:213
#7  0x00000000004951a1 in url_open_dyn_buf_internal (max_packet_size=0, s=0x7fffffffd138) at /home/realworld/libav-12.3/libavformat/aviobuf.c:1128
#8  avio_open_dyn_buf (s=s@entry=0x7fffffffd138) at /home/realworld/libav-12.3/libavformat/aviobuf.c:1145
#9  0x000000000045bc3d in configure_input_filter (in=0x164d500, ifilter=0x164c3c0, fg=0x164c2a0) at /home/realworld/libav-12.3/avconv_filter.c:673
#10 configure_filtergraph (fg=fg@entry=0x164c2a0) at /home/realworld/libav-12.3/avconv_filter.c:727
#11 0x000000000045fdc0 in ifilter_send_frame (frame=0x162b440, ifilter=0x164c3c0) at /home/realworld/libav-12.3/avconv.c:1240
#12 decode_video (ist=ist@entry=0x16424e0, pkt=pkt@entry=0x7fffffffd670, got_output=got_output@entry=0x7fffffffd608, decode_failed=decode_failed@entry=0x7fffffffd60c) at /home/realworld/libav-12.3/avconv.c:1427
#13 0x000000000044c880 in process_input_packet (no_eof=0, pkt=0x7fffffffd610, ist=0x16424e0) at /home/realworld/libav-12.3/avconv.c:1514
#14 process_input () at /home/realworld/libav-12.3/avconv.c:2690
#15 transcode () at /home/realworld/libav-12.3/avconv.c:2732
#16 main (argc=argc@entry=7, argv=argv@entry=0x7fffffffdd08) at /home/realworld/libav-12.3/avconv.c:2905
#17 0x00007ffff6fed840 in __libc_start_main (main=0x44b670 <main>, argc=7, argv=0x7fffffffdd08, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffdcf8) at ../csu/libc-start.c:291
#18 0x000000000044eac9 in _start ()
```