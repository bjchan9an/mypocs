To trigger this bug, use:

```sh
./avconv -y -i ./poc -f null -
```

The gdb crash trace is:

```
avconv version 12.3, Copyright (c) 2000-2018 the Libav developers
  built on Sep  5 2022 21:26:04 with gcc 7 (Ubuntu 7.5.0-3ubuntu1~18.04)
Ignoring attempt to set invalid timebase for st:0
[ape @ 0x5555567e76e0] packet size is not a multiple of 4. extra bytes at the end will be skipped.

Program received signal SIGSEGV, Segmentation fault.
0x0000555555bded7c in ff_scalarproduct_and_madd_int16_sse2.loop () at /home/realworld/libav-12.3/libavcodec/x86/apedsp.asm:76
76	SCALARPRODUCT
(gdb) set args -y -i ./id000011 -f null -Quit
(gdb) bt
#0  0x0000555555bded7c in ff_scalarproduct_and_madd_int16_sse2.loop () at /home/realworld/libav-12.3/libavcodec/x86/apedsp.asm:76
#1  0x0000555555728dd4 in do_apply_filter (version=3968, f=f@entry=0x5555567e8df0, data=data@entry=0x555556805c40, count=<optimized out>, count@entry=4608, order=order@entry=13, 
    fracbits=0, ctx=<optimized out>) at /home/realworld/libav-12.3/libavcodec/apedec.c:1286
#2  0x0000555555729002 in apply_filter (fracbits=0, order=13, count=4608, data1=<optimized out>, data0=<optimized out>, f=0x5555567e8dc8, ctx=0x5555567e83e0)
    at /home/realworld/libav-12.3/libavcodec/apedec.c:1346
#3  ape_apply_filters (count=4608, decoded1=0x555556805c40, decoded0=0x555556801440, ctx=0x5555567e83e0) at /home/realworld/libav-12.3/libavcodec/apedec.c:1357
#4  predictor_decode_stereo_3950 (ctx=0x5555567e83e0, count=4608) at /home/realworld/libav-12.3/libavcodec/apedec.c:1189
#5  0x000055555572e603 in ape_unpack_stereo (count=4608, ctx=0x5555567e83e0) at /home/realworld/libav-12.3/libavcodec/apedec.c:1413
#6  ape_decode_frame (avctx=<optimized out>, data=<optimized out>, got_frame_ptr=<optimized out>, avpkt=<optimized out>) at /home/realworld/libav-12.3/libavcodec/apedec.c:1537
#7  0x00005555559ca0b8 in avcodec_decode_audio4 (avctx=avctx@entry=0x5555567e76e0, frame=0x5555567e8180, got_frame_ptr=got_frame_ptr@entry=0x7fffffffd3e4, 
    avpkt=avpkt@entry=0x7fffffffd440) at /home/realworld/libav-12.3/libavcodec/utils.c:1653
#8  0x00005555559cd011 in do_decode (pkt=0x7fffffffd440, avctx=0x5555567e76e0) at /home/realworld/libav-12.3/libavcodec/utils.c:1732
#9  avcodec_send_packet (avctx=avctx@entry=0x5555567e76e0, avpkt=<optimized out>, avpkt@entry=0x7fffffffd440) at /home/realworld/libav-12.3/libavcodec/utils.c:1804
#10 0x00005555556f5e9a in try_decode_frame (st=st@entry=0x5555567e7020, avpkt=avpkt@entry=0x7fffffffd530, options=<optimized out>, s=0x5555567e67c0)
    at /home/realworld/libav-12.3/libavformat/utils.c:1950
#11 0x00005555556fa35b in avformat_find_stream_info (ic=0x5555567e67c0, options=0x5555567e7ba0) at /home/realworld/libav-12.3/libavformat/utils.c:2356
#12 0x000055555560268b in open_input_file (o=o@entry=0x7fffffffd9a0, filename=<optimized out>) at /home/realworld/libav-12.3/avconv_opt.c:771
#13 0x0000555555604815 in open_files (l=0x5555567d4058, l=0x5555567d4058, open_file=0x555555602320 <open_input_file>, inout=0x555555ce3104 "input")
    at /home/realworld/libav-12.3/avconv_opt.c:2380
#14 avconv_parse_options (argc=argc@entry=7, argv=argv@entry=0x7fffffffe4d8) at /home/realworld/libav-12.3/avconv_opt.c:2417
#15 0x00005555555f7838 in main (argc=7, argv=0x7fffffffe4d8) at /home/realworld/libav-12.3/avconv.c:2883
```