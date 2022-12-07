### Build
```bash
npm run build
```

Update `webpack.config.js` to switch to another optimization option & re-run `npm run build`.
- `imagemin`: `1.3M`
- `squoosh`: `1.2M`

### Optimize with imagemin
```js
const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");

module.exports = {
  module: {
    rules: [
      {
        test: /\.(jpe?g|png|gif|svg)$/i,
        type: "asset",
      },
    ],
  },
  optimization: {
    minimizer: [
      "...",
      new ImageMinimizerPlugin({
        minimizer: {
          implementation: ImageMinimizerPlugin.imageminMinify,
          options: {
            // Lossless optimization with custom option
            // Feel free to experiment with options for better result for you
            plugins: [
              ["gifsicle", { interlaced: true }],
              ["jpegtran", { progressive: true }],
              ["optipng", { optimizationLevel: 5 }],
              // Svgo configuration here https://github.com/svg/svgo#configuration
              [
                "svgo",
                {
                  plugins: [
                    {
                      name: "preset-default",
                      params: {
                        overrides: {
                          removeViewBox: false,
                          addAttributesToSVGElement: {
                            params: {
                              attributes: [
                                { xmlns: "http://www.w3.org/2000/svg" },
                              ],
                            },
                          },
                        },
                      },
                    },
                  ],
                },
              ],
            ],
          },
        },
      }),
    ],
  },
};
```


### Optimize with squoosh
```js
const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");

module.exports = {
  module: {
    rules: [
      // You need this, if you are using `import file from "file.ext"`, for `new URL(...)` syntax you don't need it
      {
        test: /\.(jpe?g|png)$/i,
        type: "asset",
      },
    ],
  },
  optimization: {
    minimizer: [
      "...",
      new ImageMinimizerPlugin({
        minimizer: {
          implementation: ImageMinimizerPlugin.squooshMinify,
          options: {
            // Your options for `squoosh`
          },
        },
      }),
    ],
  },
};
```