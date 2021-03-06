# go-cptv

[![Build Status](https://api.travis-ci.com/TheCacophonyProject/go-cptv.svg?branch=master)](https://travis-ci.com/TheCacophonyProject/go-cptv)

This package implements a Go package for generating and consuming
Cacophony Project Thermal Video (CPTV) files. For more details on
these files see the specifications:

* v1: (https://github.com/TheCacophonyProject/go-cptv/blob/master/SPECv1.md)
* v2: (https://github.com/TheCacophonyProject/go-cptv/blob/master/SPECv2.md) (implementation in progress)

## Example Usage

### Writing CPTV Files

```go

import (
    "github.com/TheCacophonyProject/go-cptv"
    "github.com/TheCacophonyProject/go-cptv/cptvframe"
)

type TestCamera struct {
}

func (cam *TestCamera) ResX() int {
    return 200
}

func (cam *TestCamera) ResY() int {
    return 20
}

func writeFrames(frames []*cptvframe.Frame) error {
    camera := new(TestCamera)
    w := cptv.NewFileWriter("out.cptv", camera)
    defer w.Close()
    err := w.WriterHeader("device-name")
    if err != nil {
        return err
    }
    for _, frame := range frames {
        err := w.WriteFrame(frame)
        if err != nil {
            return err
        }
    }
    return nil
}
```

### Reading CPTV Files

See [cptvtool](https://github.com/TheCacophonyProject/go-cptv/tree/master/cptvtool) for a read example.
