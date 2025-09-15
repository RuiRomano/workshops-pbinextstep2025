# pbir-themefix

This file defines instructions on how to find static colors with hex colors in a Power BI report using PBIR file format and replace them to use a theme color.

## Plan

- Enumerate all the visual files, search for files with name `visual.json`
- Search for color configurations that reference a color using a statix HEX and not a theme.

    **Example Using HEX color**: #6E73D9

    ```json
        ...
        "dataPoint": [
                {
                "properties": {
                    "fill": {
                    "solid": {
                        "color": {
                            "expr": {
                                "Literal": {
                                "Value": "'#6E73D9'"
                                }
                            }
                        }
                    }
                    }
                }
                }
            ],
        ...
    ```
- After identifying all the visuals with hex colors, build a mapping of these colors to one of the 8 colors of the theme. 
    
    Power BI theme colors have 8 colors plus black and while.

    Black and white are ColorId 0 and 1
    The remaining theme colors are ColorId 2 to 10

- Replace the HEX color static reference for a theme color


    **Example Using theme color**: ColorId = 2
    ```json
        ...
        "dataPoint": [
                {
                "properties": {
                    "fill": {
                    "solid": {
                        "color": {
                            "expr": {
                                "ThemeDataColor": {
                                    "ColorId": 2,
                                    "Percent": 0
                                }
                            }
                        }
                    }
                    }
                }
                }
            ],
        ...
    ```