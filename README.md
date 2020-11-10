# Kanji-stroke-order
SVG files for Wanikani kanji stroke order

This is a collection of files containing SVG images of stroke order for drawing kanji. The data originally come from Ulrich Apel's KanjiVG project. (http://kanjivg.tagaini.net/)
The data is freely available under the Creative Commons Attribution-Share Alike 3.0 license. 

https://creativecommons.org/licenses/by-sa/3.0/legalcode

Jisho.org used this data to produce the SVG files. Their work is relesed here and is also available under the Creative Commons Attribution-Share Alike 3.0 license.

http://forum.jisho.org/discussion/comment/4699

I have collected these files in objects indexed by their kanji. These objects are under the JSON format. They are available also under the Creative Commons Attribution-Share 
Alike 3.0 license.

The original files are too large to be hosted on github. They are available in LZMA compressed format.

* All_svg.json.compressed          - All SVG files provided by jisho.org.
* WK_svg_normal.json.compressed    - The SVG for the kanji taught by wanikani.com in their original height of 109px
* WK_svg_smaller.json.compressed   - Same as the previous file but proportionally smaller with a reduced height of 63px.

LZMA is a binary format. In the context of a Tampermonkey userscript is not always possible to transfer binary files over the network. Versions of these three LZMA files 
are available encoded into base64 format. These are ascii files that will pass where binary is prohibited.

* All_svg.json.base64          - All SVG files provided by jisho.org.
* WK_svg_normal.json.base64    - The SVG for the kanji taught by wanikani.com in their original height of 109px
* WK_svg_smaller.json.base64   - Same as the previous file but proportionally smaller with a reduced height of 63px.

The SVG in these file differ from the jisho original in that the px units are omitted from the viewBox attribute. This change is inconsequential because when units are 
missing from the viewBox attributes SVG defaults to px. This change is required because my need for these files is to use them in a wanikani.com userscript. There is 
some Javascript on wanikani that reacts negatively to the presence of units in viewBox attributes and generates error messages on the Javascript console. Removing the 
units works around this problem.

Using these files is a multistep process.

* If you use the base64 versions you need to decode them to produce the LZMA binary version.
* Then you need to decompress them to obtain the JSON data.
* Then you need to parse JSON to retrieve the kanji-indexed object.

Decoding based64 is done with the following algorithm. (Javascript) This function produces an array of bytes ready for decompression. The array may be turned into a binary
file if needed.

<code>
    
    function base64ToBytesArr(str) {
    
        const abc = [..."ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"]; // base64 alphabet
    
        let result = [];

        for(let i=0; i<str.length/4; i++) {
            let chunk = [...str.slice(4*i,4*i+4)]
            let bin = chunk.map(x=> abc.indexOf(x).toString(2).padStart(6,0)).join('');
            let bytes = bin.match(/.{1,8}/g).map(x=> +('0b'+x));
            result.push(...bytes.slice(0,3 - (str[4*i+2]=="=") - (str[4*i+3]=="=")));
        }
        return result;
     };
</code>

For decompression you may use:

* On Windows the LZMA utility available here will work. This is the lzma.exe file buried in the 7-zip archive. Just copy it to your working directory.

https://www.7-zip.org/sdk.html

* In a Tampermonkey userscript use jcmellado's js-lzma javascript port of LZMA.

https://github.com/jcmellado/js-lzma
