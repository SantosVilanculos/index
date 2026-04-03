## use

### embedded thumbnail

```sh
ffmpeg -i ./input.mp4 -i ./input.png -map 0 -map 1 -c copy -c:v:1 png -disposition:v:1 attached_pic ./output.mp4
```
