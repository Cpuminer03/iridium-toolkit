# Simple toolkit to decode Iridium signals

### Requisites

 * Python (2.7)
 * NumPy (scipy)

### License

Unless otherwise noted in a file, everything here is (c) Sec & schneider and licensed under the 2-Clause BSD License

### Example usage
Either extract some Iridium frames from the air or a file using [gr-iridium](https://github.com/muccc/gr-iridium) (recommended) or use the legacy code located in the [extractror-python](extractor-python/) directory if you don't want to install GNURadio (not recommended).

Is is assumed that the output of the extractor has been written to `output.bits`. Iridium frames can be decoded with

    python2 iridium-parser.py output.bits

if you want to speed up that step you can install `pypy` and instead run 

    pypy iridium-parser.py output.bits

### Frame extraction
See  [gr-iridium](https://github.com/muccc/gr-iridium) (recommended) or [extractror-python](extractor-python/) (not recommended) on how to extract Iridium frames from raw data.

### Voice Decoding
To listen to voice calls, you will need an AMBE decoder. There are two option:
 - Use tnt's open source AMBE decoder: http://git.osmocom.org/osmo-ir77/tree/codec (`git clone http://git.osmocom.org/osmo-ir77`)
 - Extract an AMBE decoder from a firmware binary. Have a look at the [documentation](ambe_emu/Readme.md) in the `ambe_emu/` directory.

The easier option is to use tnt's AMBE decoder. You can use the extracted decoder if you want to create bit correct output. There almost no audible difference between the two options. Make sure that either `ir77_ambe_decode` or `ambe` is in your `PATH`. Also select the installed one in `play-iridium-ambe`.

Make sure that the main folder of the toolkit is in your `PATH` variable: `export PATH=$PATH:<this directory>`

Steps to decode voice:
 - Decode your captured and demodulated bits using `iridium-parser` and put the result into a file: `pypy iridium-parser.py output.bits > output.parsed`
 - Use `voc-stats.py` to see streams of captured voice frames: `./voc-stats.py  output.parsed`
 - Click once left and once right to select an area. `voc-stats.py` will try do decode and play the selected samples using the `play-iridium-ambe` script.


### Main Components

#### Parser

`iridium-parser.py`

Takes the demodulated bits and tries to parse them into a readable format.

Supports some different output formats (`-o` option).

