# VietIME for jQuery

**Vietnamese IME for Quoc Ngu and Chu Han Nom**

VietIME allows Vietnamese typing on HTML input fields and textareas (supports Telex and VNI).

Chu **Han-Nom** conversion is supported if *candidate_list* is set to *true*.

See a working example:
https://www.chunom.org/pages/ime/

## Usage HTML
	
    <script src="jquery.js"></script>
    <script src="vietime.js"></script>
    <input id="input"/>

## Usage JavaScript

    // Quoc-ngu only using Telex
    $("#input").vietime({method: 1});
    
    // Quoc-ngu only using VNI
    $("#input").vietime({method: 2});

    // Quoc-ngu only using Telex and VNI
    $("#input").vietime({method: 4});

    // Support Han-Nom and Quoc-ngu using Telex
    $("#input").vietime({ candidate_list: true });

## Options

### candidate_list [false]

Enable Han-Nom conversion.
        
### defer [false]

Initializes IME only when element is focused.
        
### return_select [false]

Enable return key as Han-Nom selector
    
### space_select [false]

Enable space key as Han-Nom selector
    
### dictionaries ['//www.chunom.org/entry/generated_chars/']

More JSON dictionaries to load when using Chu Han Nom. These files are not cached and therefore loaded on every page load. This is the place where you can add your own Han Nom characters. Dictionaries can be inline or outsourced to external JSON files.

For the structure of dictionaries see chapter **Defining a dictionary structure**.

### cached_dictionary ['//www.chunom.org/entry/base_chars/']

The base JSON dictionary containing many characters from Unicode and other sources.
This JSON file is loaded only once and then saved **permanently** in browser cache.
You can only update the cache by changing the URL.
Set to null to disable this dictionary.

### show_defaults [false]

Show list of default symbols if no candidates are matched
        
### show_definitions [true]

Show character definitions if available (only when *candidate_list* is enabled)
    
### list_changed [null]

Handler function for candidate list change event. You can use this handler to implement you own candidate list display.

Example:

    $("#input").vietime({
        candidate_list: true,
        list_changed: function(container, items, page, pages, candidates){
	    console.log('Found ' + items + ' candidates:', candidates)
	}
    })

        
### candidate_selected [null]

Handler function for candidate selection event. Passed parameters are: glyph, typed_word.
*glyph* is what the user selected from the candidate list. *typed_word* is what the user typed and will be replaced by *glyph*.
The handler function must return *glyph* or an other string to replace *glyph*.

Example:

    $("#input").vietime({
        candidate_list: true,
        candidate_selected: function(glyph, typed_word){
	    console.log('The word ' + typed_word + ' will be replaced by ' + glyph);
	    return glyph;
	}
    })


### font_list [""]

Font stack to use for character display in candidate list.
Can be a comma separated string or an array of strings.
E.g. "SimSun-ExtB, Heiti TC" or ["SimSun-ExtB", "Heiti TC"]
        
### match_fonts [true]

Applies font stack used by candidate list to input field.
Only when candidate_list=true
        
### background ['#eeeeee']

Background color of candidate list
    
### glyph_color [null]

Text color of CJK characters in candidate list.
defaults to text_color
        
### text_color ['#000000']

Text color in candidate list
    
### autocomplete_color ['#0000ff']

Color of auto completed text in candidate list
        
### border ['2px solid #000000']

CSS border style of candidate list
        
### grid_color ['#333333']

Color of separator between candidates

### hover_color ['rgba(10, 120, 255, .4)']

Background color of the candidate under the mouse pointer

### glyph_hover_background ['white']

Background behind the glyph of the hovered candidate. Set to a dark color when using light *glyph_color* on a dark page.

### definition_hover_background ['white']

Background of an expanded definition when hovered

### padding [2]

CSS padding of candidates

### position ['bottom']

Position of candidate list relative to input element.
Possible values: float, top, bottom, left, right
        
### offsetX [0]

Horizontal offset in pixels of candidate list, relative to the position specified by "position"

### offsetY [0]

Vertical offset in pixels of candidate list, relative to the position specified by "position"
    
### method [true]

Enable Vietnamese typing method.
If candidate_list is disabled, Telex and VNI are recognized.
If candidate_list is enabled, Telex is recognized as typing method.

## Defining a dictionary stucture

A dictionary is an object with the structure *{**dict**: '...', **defs**: {...}}*.

**dict** is a long string containing your character entries, each separated by the pipe symbol "|". Each character entry starts with the Han Nom character followed by a colon (:) then followed by the Quoc Ngu pronunciations separated by comma (,). For example *"固:có,cố|别:biết,biệt|爫:làm"*

**defs** is optional, if present it points to a mapping object of pronunciation / definition pairs. Pronunciations with more than one syllable can be written in camel case to save space, e.g. việtNam, chữNôm.

Example:

    $('#input').vietime({
        candidate_list: true,
        dictionaries: [
            // Loads the default dictionary of chunom.org
            "//www.chunom.org/entry/generated_chars/",
            
            // Add your own characters and definitions
            {
                'dict': "固:có,cố|别:biết,biệt|爫:làm",
                'defs': {
                    'có': "to have",
                    'việtNam': "Viet Nam (country)",
                    'chữNôm': "The former writing system of Viet Nam"
                }
            }
        ],
    })

