[![CircleCI](https://circleci.com/gh/guitarrapc/SkiaSharp.QrCode.svg?style=svg)](https://circleci.com/gh/guitarrapc/SkiaSharp.QrCode) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE) [![NuGet](https://img.shields.io/nuget/v/SkiaSharp.QrCode.svg)](https://www.nuget.org/packages/SkiaSharp.QrCode)

## Skia.QrCode

Qr Code generator with [Skia.Sharp](https://github.com/mono/SkiaSharp).

## Install

.NET CLI

```
$ dotnet add package SkiaQrCode
```

Package Manager

```
PM> Install-Pacakge Skia.QrCode
```

## Motivation

There are many ZXing.Net + System.Drawing samples to generate Qr.
If you want avoid System.Drawing, you may use [ImageSharp](https://github.com/SixLabors/ImageSharp) or [Core.Compat.System.Drawing](https://github.com/CoreCompat/System.Drawing).

However using these code required much coding, I just want to create QR!

## Why Skia?

> [.NET Core Image Processing](https://blogs.msdn.microsoft.com/dotnet/2017/01/19/net-core-image-processing/)

## Sample Code

Here's minimum sample to generate specific qrcode via args.

```csharp
using SkiaSharp;
using SkiaSharp.QrCode.Image;
using System;
using System.IO;

namespace SimpleGenerate
{
    class Program
    {
        static void Main(string[] args)
        {
            if (args.Length < 2)
            {
                Console.WriteLine("Usage: dotnet SimpleGenerate.dll your_message output_path");
                return;
            }
            var content = args[0];
            var path = args[1];

            // generate qr code
            var qrCode = new QrCode(content, new Vector2Slim(256, 256), SKEncodedImageFormat.Png);
            using (var output = new FileStream(path, FileMode.OpenOrCreate))
            {
                qrCode.GenerateImage(output);
            }

            Console.WriteLine($"Successfully output QRCode in {path}");
        }
    }
}

```

If you want specify detail, you can generate manually.

```csharp
using SkiaQrCode;
using SkiaSharp;
using System;
using System.IO;

namespace SkiaQrCodeSampleConsole
{
    class Program
    {
        static void Main(string[] args)
        {
            var content = "testtesttest";
            using (var generator = new QRCodeGenerator())
            {
                // Generate QrCode
                var qr = generator.CreateQrCode(content, ECCLevel.L);

                // Render to canvas
                var info = new SKImageInfo(512, 512);
                using (var surface = SKSurface.Create(info))
                {
                    var canvas = surface.Canvas;
                    canvas.Render(qr, info.Width, info.Height);

                    // Output to Stream -> File
                    using (var image = surface.Snapshot())
                    using (var data = image.Encode(SKEncodedImageFormat.Png, 100))
                    using (var stream = File.OpenWrite(@"output/hoge.png"))
                    {
                        data.SaveTo(stream);
                    }
                }
            }
        }
    }
}

```

## Build

```
docker build -t skiasharp.qrcode .
```

## License

MIT

## Thanks

> [aloisdeniel/Xam.Forms.QRCode](https://github.com/aloisdeniel/Xam.Forms.QRCode) : Qr Sample with Skia
> [codebude/QRCoder](https://github.com/codebude/QRCoder) : all QRCode generation algorithms