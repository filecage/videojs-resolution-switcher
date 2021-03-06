# Video.js Resolution Switcher

Resolution switcher for [video.js v5](https://github.com/videojs/video.js)

## Getting Started

Install plugin with

**npm**
```
npm i videojs-resolution-switcher
```

or **bower**
```
bower install videojs-resolution-switcher
```


### Setup sources dynamically:

```html
<video id='video' class="video-js vjs-default-skin"></video>
<script src="video.js"></script>
<script src="videojs-resolution-switcher.js"></script>
<script>
  videojs('video', {
    controls: true,
    plugins: {
        videoJsResolutionSwitcher: {
          default: 'high',
          dynamicLabel: true
        }
      }
  }, function(){
  
    // Add dynamically sources via updateSrc method
    player.updateSrc([
        {
          src: 'http://media.xiph.org/mango/tears_of_steel_1080p.webm',
          type: 'video/webm',
          label: '360'
        },
        {
          src: 'http://mirrorblender.top-ix.org/movies/sintel-1024-surround.mp4',
          type: 'video/mp4',
          label: '720'
        }
      ])

      player.on('resolutionchange', function(){
        console.info('Source changed to %s', player.src())
      })
      
  })
</script>
```

### Or use `<source>` tags:

```html

<video id="video" class="video-js vjs-default-skin" width="1000" controls data-setup='{}'>
   <source src="http://mirrorblender.top-ix.org/movies/sintel-1024-surround.mp4" type='video/mp4' label='SD' />
   <source src="http://media.xiph.org/mango/tears_of_steel_1080p.webm" type='video/webm' label='HD'/>
</video>
<script>
  videojs('video').videoJsResolutionSwitcher()
</script>

```

### Flash tech

When using flash tech `preload="auto"` is required.

## Source options

Sources can passed to player using `updateSrc` method or `<source>` tag as shown above. Avalible options for each source are:
* label - `String` (required) is shown in menu (ex. 'SD' or '360p')
* res - `Number` is resolution of video used for sorting (ex. 360 or 1080)

## Plugin options

You can pass options to plugin like this:

```javascript

videojs('video', {
      controls: true,
      muted: true,
      width: 1000,
      plugins: {
        videoJsResolutionSwitcher: {
          default: 'low'
        }
      }
    }, function(){
      // this is player
    })
```
### Avalible options:
* default - `{Number}|'low'|'high'` - default resolution. If any `Number` is passed plugin will try to choose source based on `res` parameter. If `low` or `high` is passed, plugin will choose respectively worse or best resolution (if `res` parameter is specified). If `res` parameter is not specified plugin assumes that sources array is sorted from best to worse.
* dynamicLabel - `{Boolean}` - if `true` current label will be displayed in control bar. By default gear icon is displayed.

## Example

[Working example](example.html) of the plugin you can check out if you're having trouble. Or check out this [demo](https://kmoskwiak.github.io/videojs-resolution-switcher/).

## Methods


### updateSrc([source])
Returns video.js player object if used as setter. If `source` is not passed it acts like [player.src()](http://docs.videojs.com/docs/api/player.html#Methodssrc)
```javascript

// Update video sources
player.updateSrc([
  { type: "video/mp4", src: "http://www.example.com/path/to/video.mp4", label: 'SD' },
  { type: "video/mp4", src: "http://www.example.com/path/to/video.mp4", label: 'HD' },
  { type: "video/mp4", src: "http://www.example.com/path/to/video.mp4", label: '4k' }
])

```
#### PARAMETERS:
| name | type | required | description |
|:----:|:----:|:--------:|:-----------:|
| source| array| no | array of sources |

### currentResolution([label])
If used as getter returns current resolution object:
```javascript
  {
    label: 'SD', // current label
    sources: [
      { type: "video/webm", src: "http://www.example.com/path/to/video.webm", label: 'SD' },
      { type: "video/mp4", src: "http://www.example.com/path/to/video.mp4", label: 'SD' }
    ] // array of sources with current label
  }
```

If used as setter returns video.js player object.


```javascript

// Get current resolution
player.currentResolution(); // returns object {label '', sources: []}

// Set resolution
player.currentResolution('SD'); // returns videojs player object
```
#### PARAMETERS:
| name | type | required | description |
|:----:|:----:|:--------:|:-----------:|
| label| string| no | label name |


## Events

### resolutionchange `EVENT`

> Fired when resolution is changed


