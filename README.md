# Enhancing the `Sootty` Terminal-based Graphical Waveform Viewer

Google Summer of Code (GSoC) 2022 Free and Open Source Silicon (FOSSi) Foundation Project Report

## Introduction

Sootty is a terminal-based graphical waveform viewer. It relies on `pyvcd` to read and parse the Value Change Dump (VCD) files and `lark` to provide a command-line query language to specify start points, end points, breakpoints, etc. To display output SVG pictures, only `kitty` and `iterm` terminals are applicable.

![img](sootty-main.drawio.png)

## Contribution

### Milestone 1: **Add Numeral System**

Code: [Merged] [Pull requests #46 · Ben1152000/sootty](https://github.com/Ben1152000/sootty/pull/46)

Deliverables:

- New argument`-r` `—-radix` : Display values in radix N (N=2~36, default 10)
- Helper function `dec2anybase`
- Documentation

```
def dec2anybase(input, base, width):
    """
    Convert a decimal into any base (2 - 36).
    Trailing zeros are added according to input value bit size (width).
    """
    res = str()
    while input > 0:
        rem = input % base
        if rem >= 0 and rem <= 9:
            res += chr(rem + ord("0"))
        else:
            res += chr(rem - 10 + ord("A"))
        input = input // base
    return res[::-1].zfill(ceil(log(2**width - 1, base)))
```

### Milestone 2: Add EVCD to VCD Converter

Code: [Merged] [Pull requests #52 · Ben1152000/sootty](https://github.com/Ben1152000/sootty/pull/52), [Merged] [Pull requests #55 · Ben1152000/sootty](https://github.com/Ben1152000/sootty/pull/55)

Deliverables:

- Helper function `vcdid_hash` , `vcdid_unhash` , and `evcd_strcpy` .
- Converter function: `evcd2vcd`
- New examples `example4.evcd` , `IEEE_std_example.vcd` , `IEEE_std_example.evcd`
- Documentation

### Milestone 3: Add Breakpoints Colors and Table Printer

Code: [Pull requests #59 · Ben1152000/sootty](https://github.com/Ben1152000/sootty/pull/59), [Pull requests #61 · Ben1152000/sootty](https://github.com/Ben1152000/sootty/pull/61), [Pull Request #64 · Ben1152000/sootty](https://github.com/Ben1152000/sootty/pull/64)

Deliverables:

- `BREAKPOINT_COLOR_LIST` in `Style` of `Visualizer` class
- Modified class function `get_names` of `WireGroup` class
- New class function `get_wires` of `WireGroup` class
- New class function `print_breakpoints` of `WireTrace` class
- Remove redundant imports
- New argument `--btable`
- Documentation

**Others**

- Code reviews of peers’ work
- Fix encoding bug: [Merged] [Pull Request #63 · Ben1152000/sootty](https://github.com/Ben1152000/sootty/pull/63)

## Discussions

### Challenges

1. The "X" or "x" values need to be carefully considered when converting radix. The preceding zeros are complemented according to the wire widths except for the default radix 10.
2. The referenced `evcd2vcd` done by `gtkwave` written in C has some problems and issues, so our work needs to re-implement in a pythonic way passing different test examples.
3. If `-b` and `--btable` are given simultaneously, to print the breakpoint table is done after displaying the SVG by `Popen`.
4. The function `print_breakpoints` traverse all the wires by `get_wires` recursively once to print the wire information directly. If a better formatting is needed, the column widths have to be determined by the maximum values of each columns through `get_names`, so that the wire groups needs to be traversed twice. If we want to determine the hierarchy of scopes to apply dot references like `testbench.pe8` when printing, the wire groups also needs to be traverse once more.

### Learnings

1. Practiced using Git and GitHub more
2. Improved my communication and cooperation skills on remote
3. Have developed a deeper understanding of waveform formats
4. Learned a lot about object oriented programming (OOP) in Python
5. Learned a lot about Python `io` library and miscellaneous stuff

### Future Work 

1. Add scope information in the output SVG
2. Explore the opportunities in extending the compatibility across different platforms
3. Explore an appropriate way to support more complex waveform formats like `fsdb`
4. Find a smart and efficient way to format the breakpoint table prettier and apply dot references on scopes.

## Conclusion

To conclude, I have finished all three tasks planned in my original proposal. I feel lucky to be able to participate in GSoC and really appreciate my mentors, Jonathan Balkind and Benjamin Darnell, for their help. I really enjoyed this program during the past three months. This project inspires me to go on to explore and contribute to the world of free and open-source silicon in the future.
