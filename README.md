# How to generate Korean fonts for u8g2

NOTE: If the fonts come out garbled, please use `u8g2.enableUTF8Print();` inside the `void setup() {}` function or check the font size.

Please start from step {step} to skip the mapping file generation and jump straight to generating the fonts.

1. Prepare a TTF or OTF font file that has the characters you need (invalid characters will cause parsing errors)
2. Type out all letters you need, separated with a space or other special characters (i.e. "반갑습니다 전원 켜기 끄기 옵션 설정")
3. Then you need to insert line breaks to every character in the line, use regex if it's a lot, or just do it manually
4. Using a recent version of Notepad++, remove duplicate lines (this is why we used line breaks to separate the characters)
5. Sort lines lexicologically 
6. Ctrl+A to select all
7. Ctrl+J to join all lines with `0x20` (space character) between them
8. Open in hex editor
9. Copy as hex bytes and put it in the converter program (will be released later, it just converts Unicode data into Unicode code page entry)
10. You'll have something that looks like: `0xAC00, 0xB098, 0xB2E4`
11. Save as `mymap.map`
12. Add `0x0021, 0x007E,` at the beginning
13. Make font at first using commands:
```
otf2bdf.exe -r 72 "FontName.ttf" -o "FontName-16p.bdf" -p 16
otf2bdf.exe -r 72 "FontName.ttf" -o "FontName-24p.bdf" -p 24
otf2bdf.exe -r 72 "FontName.ttf" -o "FontName-36p.bdf" -p 36
```
14. Copy the resulting BDF files to the folder that has `bdfconv.exe` inside
15. Run `bdfconv -f 1 -M mymap.map -n %1 -o "FontName.c" "FontName.bdf"`
16. Rename the resulting file, `FontName.c` to `FontName.h`
17. Import into Arduino with `#import "FontName.h"`
