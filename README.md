# sprigganjs

Sprite designer frontend the [pixie](https://github.com/cagedev/pixie) LED-panel server.

## Model / options
- SprigganPanel {pixel_data, width, height}
  Keeps track of data.
  - AutoCanvas / Thumbnail (output, {autoupdate=true|TODO:update_event}) {pixel_data, width, (height [tecnically not required])}
    Draws pixel data straight as is. Should be quick as it uses canvas.context.putImageData
  - PixelCanvas (output / TODO:input-> inputLayer)
    Draws every pixel as a 
  - InputLayer (TODO:input)
    (invisible) mouse capture input layer that translates clicks & moves to events to be captured by the PixiePanel (should pixiepanel only track pixel data)
  - PixieOutput (TODO:output) {pixel_data, width, height, TODO:pixie_api_endpoint, TODO:update_method{pixel_array|single_pixel?|image_file?} }
    Updates an external Pixie-server with the pixel data.
  - Palette
  - ButtonBar

## Status

Work-in-progress:

### TODO:
- [x] image loading
- [x] local image saving (screenshot-style)
- [x] internal image model
- [ ] local image saving (thumbnail style)
- [ ] live example (gh-pages)
- [ ] post to pixie
- [ ] live connection to pixie (websocket)


## NPM Commands
- Project setup `npm install`
- Compiles and hot-reloads for development `npm run serve`
- Compiles and minifies for production `npm run build`
- Lints and fixes files `npm run lint`
- Customize configuration See [Configuration Reference](https://cli.vuejs.org/config/).
