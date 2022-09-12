To trigger this bug, use:
```sh
avconv -y -i ./poc -f null -
```

GDB trace:

```sh
[aac @ 0x5555567e67c0] Format detected only with low score of 1, misdetection possible!
[aac @ 0x5555567e74c0] Sample rate index in program config element does not match the sample rate index configured by the container.
[aac @ 0x5555567e74c0] Number of bands (34) exceeds limit (20).

Program received signal SIGSEGV, Segmentation fault.
ff_sbr_hf_gen_sse.loop2 () at /home/realworld/libav-12.3/libavcodec/x86/sbrdsp.asm:182
182	    mova  [X_highq + start], m5
(gdb) bt

#0  ff_sbr_hf_gen_sse.loop2 () at /home/realworld/libav-12.3/libavcodec/x86/sbrdsp.asm:182
#1  0x0000555555ae5732 in sbr_hf_gen (ac=0x7ffff7ec8240, bs_num_env=0, t_env=0x7ffff7eda42c "", bw_array=0x7ffff7ec86cc, alpha1=0x7ffff7f00d80, alpha0=0x7ffff7f00b80, 
    X_low=0x7ffff7eefb80, X_high=0x7ffff7ef2380, sbr=0x7ffff7eaf720) at /home/realworld/libav-12.3/libavcodec/aacsbr.c:1348
#2  ff_sbr_apply (ac=ac@entry=0x5555567e8300, sbr=sbr@entry=0x7ffff7eaf720, id_aac=id_aac@entry=1, L=<optimized out>, R=<optimized out>)
    at /home/realworld/libav-12.3/libavcodec/aacsbr.c:1683
#3  0x0000555555ad3f58 in spectral_to_sample (ac=ac@entry=0x5555567e8300) at /home/realworld/libav-12.3/libavcodec/aacdec.c:2692
#4  0x0000555555adb8ab in aac_decode_frame_int (avctx=avctx@entry=0x5555567e74c0, data=data@entry=0x5555567e80a0, got_frame_ptr=got_frame_ptr@entry=0x7fffffffd404, 
    gb=gb@entry=0x7fffffffd340) at /home/realworld/libav-12.3/libavcodec/aacdec.c:2945
#5  0x0000555555adbcb8 in aac_decode_frame (avctx=0x5555567e74c0, data=0x5555567e80a0, got_frame_ptr=0x7fffffffd404, avpkt=<optimized out>)
    at /home/realworld/libav-12.3/libavcodec/aacdec.c:3011
#6  0x00005555559ca0b8 in avcodec_decode_audio4 (avctx=avctx@entry=0x5555567e74c0, frame=0x5555567e80a0, got_frame_ptr=got_frame_ptr@entry=0x7fffffffd404, 
    avpkt=avpkt@entry=0x7fffffffd460) at /home/realworld/libav-12.3/libavcodec/utils.c:1653
#7  0x00005555559cd011 in do_decode (pkt=0x7fffffffd460, avctx=0x5555567e74c0) at /home/realworld/libav-12.3/libavcodec/utils.c:1732
#8  avcodec_send_packet (avctx=avctx@entry=0x5555567e74c0, avpkt=<optimized out>, avpkt@entry=0x7fffffffd460) at /home/realworld/libav-12.3/libavcodec/utils.c:1804
#9  0x00005555556f5e9a in try_decode_frame (st=st@entry=0x5555567e6e00, avpkt=avpkt@entry=0x7fffffffd550, options=<optimized out>, s=0x5555567e67c0)
    at /home/realworld/libav-12.3/libavformat/utils.c:1950
#10 0x00005555556fa35b in avformat_find_stream_info (ic=0x5555567e67c0, options=0x5555567d5a60) at /home/realworld/libav-12.3/libavformat/utils.c:2356
#11 0x000055555560268b in open_input_file (o=o@entry=0x7fffffffd9c0, filename=<optimized out>) at /home/realworld/libav-12.3/avconv_opt.c:771
#12 0x0000555555604815 in open_files (l=0x5555567d4058, l=0x5555567d4058, open_file=0x555555602320 <open_input_file>, inout=0x555555ce3104 "input")
    at /home/realworld/libav-12.3/avconv_opt.c:2380
#13 avconv_parse_options (argc=argc@entry=7, argv=argv@entry=0x7fffffffe4f8) at /home/realworld/libav-12.3/avconv_opt.c:2417
#14 0x00005555555f7838 in main (argc=7, argv=0x7fffffffe4f8) at /home/realworld/libav-12.3/avconv.c:2883
```