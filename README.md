WBC Brew-Calculator
===================

_If you need a full-fledged-and-belled-and-whistled brewing software,
look elsewhere._

WBC is homebrew software which takes in a recipe and dumps out a
printable brewday summary, as demonstrated by the [examples](#examples).
The goal of WBC is to replace pen-and-paper calculations.  The goal of
WBC is not to think for the brewer and not to be so sophisticated that
the user experience becomes a matter of outsmarting the software.

The main (and only?) features are:

  * recipe specification is so compact that you can get an idea
    of the beer with a single glance;
    see [example](#example-recipe) [recipes](#example-recipe-1)
  * input and output units configurable to both metric and cryptic,
    also SG and Plato
  * handle fermentable yields and estimated strength / ABV, and
    optionally convert grainbill percentages to masses (per target
    strength and volume)
  * calculate water temperatures and volumes for infusion mashes,
    also multi-step (decoction mass calculations are planned ... some day)
  * calculate IBUs and optionally calculate hop additions based on
    desired IBUs
  * a machine-readable compiled format -- if the recipe is what
    you want to brew, the compiled format is the ingredients you used
    to achieve what you wanted.  This data can be used to track
    ingredient usage, or be fed back into the software to see how the
    ingredients would have turned out with different system parameters
    (e.g. mash efficiency and/or boiloff rate)

The recipe interface might change in a way not backward compatible as
the software reaches final gravity.

The "documentation" is provided by the examples.  Yes, I agree,
it's not real documentation, so we'll just use the "this software is
self-documenting" bingo card.  I will write documentation once the
interface will not change.

Examples
========

<!-- BEGIN EXAMPLE -->
Example recipe
---
```
name:   MarBock
yeast:  WLP833
volume: 19.5l
boil:   90min

mashparams: { temperature: 66degC }

fermentables:
    anchor: [ strength, 16.8 degP ]

    mash:
        Avangard Pilsner:       rest
        Avangard Vienna:        15%
        Avangard Munich Light:  10%

hops:
    boil:
        - [ [ Northern Brewer, 9.9%, pellet ], 36 Recipe IBU, 90 min ]
        - [ [ Saaz,            3.1%, leaf   ], 20 g,          30 min ]
        - [ [ Hallertau,       3.8%, pellet ], 15 g,          15 min ]
```
translated with `wbctool -u metric -u plato`:
```
==============================================================================
Name:              MarBock
Final volume:      19.5l              Boil:                90 min             
IBU (Tinseth):     36.00              BUGU:                0.50               
Color (Morey):     10.9 EBC, 5.5 SRM  Water (20.0°C):      33.9l              
Pitch rate, ale:   267 billion        Pitch rate, lager:   535 billion        

Yeast:             WLP833
Water notes:       

Preboil  volume  : 27.1l (70.0°C)     Measured:                               
Preboil  strength: 13.4°P             Measured:                               
Postboil volume  : 22.2l (100.0°C)    Measured:                               
Postboil strength: 16.8°P             Measured:                               

Kettle loss (est): 1.0l               Fermenter loss (est):0.8l               
Mash eff (conf) :  88.0%              Brewhouse eff (est): 84.6%              
==============================================================================

Fermentables                                  amount  ext (100%)   ext (88%)
==============================================================================
Mash
------------------------------------------------------------------------------
Avangard Pilsner                    4.31 kg ( 75.0%)     3.27 kg     2.88 kg
Avangard Vienna                       862 g ( 15.0%)       650 g       572 g
Avangard Munich Light                 575 g ( 10.0%)       433 g       381 g
------------------------------------------------------------------------------
                                    5.75 kg (100.0%)     4.35 kg     3.83 kg
==============================================================================
                                    5.75 kg (100.0%)     4.35 kg     3.83 kg

Mashing instructions (for ambient temperature 20.0°C)
==============================================================================
66.0°C : add 14.8l of water at 77.8°C (2.50 l/kg, mash vol 18.4l)
Mashstep water volume: 14.4l @ 20.0°C
Sparge water volume:   20.1l @ 82.0°C
First wort (conv. %):  20.0°P (85%), 21.1°P (90%), 22.1°P (95%), 23.2°P (100%)
==============================================================================

Hops                              AA%                time    amount     IBUs
==============================================================================
Northern Brewer (pellet)          9.9%             90 min    25.1 g    28.36
Saaz (leaf)                       3.1%             30 min    20.0 g     4.62
Hallertau (pellet)                3.8%             15 min    15.0 g     3.02
==============================================================================
                                                             60.1 g    36.00

Speculative apparent attenuation and resulting ABV
==============================================================================
  Str.    Att.  ABV         Str.    Att.  ABV         Str.    Att.  ABV       
 7.3°P    60%   5.6%       5.5°P    70%   6.6%       3.7°P    80%   7.7%      
 6.4°P    65%   6.1%       4.6°P    75%   7.1%       2.8°P    85%   8.2%      
==============================================================================

```

translated with `wbctool -u us -u sg`:
```
==============================================================================
Name:              MarBock
Final volume:      5.2gal             Boil:                90 min             
IBU (Tinseth):     36.00              BUGU:                0.50               
Color (Morey):     10.9 EBC, 5.5 SRM  Water (68.0°F):      9.0gal             
Pitch rate, ale:   267 billion        Pitch rate, lager:   535 billion        

Yeast:             WLP833
Water notes:       

Preboil  volume  : 7.2gal (158.0°F)   Measured:                               
Preboil  strength: 1.054              Measured:                               
Postboil volume  : 5.9gal (212.0°F)   Measured:                               
Postboil strength: 1.069              Measured:                               

Kettle loss (est): 0.3gal             Fermenter loss (est):0.2gal             
Mash eff (conf) :  88.0%              Brewhouse eff (est): 84.6%              
==============================================================================

Fermentables                                  amount  ext (100%)   ext (88%)
==============================================================================
Mash
------------------------------------------------------------------------------
Avangard Pilsner                   9 1/2 lb ( 75.0%)   7 3/16 lb   6 5/16 lb
Avangard Vienna                    1 7/8 lb ( 15.0%)    1 3/8 lb    1 1/4 lb
Avangard Munich Light              1 1/4 lb ( 10.0%)    15.28 oz    13.44 oz
------------------------------------------------------------------------------
                                  12 5/8 lb (100.0%)   9 9/16 lb   8 7/16 lb
==============================================================================
                                  12 5/8 lb (100.0%)   9 9/16 lb   8 7/16 lb

Mashing instructions (for ambient temperature 68.0°F)
==============================================================================
150.8°F: add 3.9gal of water at 172.0°F (1.20 qt/lb, mash vol 4.9gal)
Mashstep water volume: 3.8gal @ 68.0°F
Sparge water volume:   5.3gal @ 179.6°F
First wort (conv. %):  1.083 (85%), 1.088 (90%), 1.093 (95%), 1.098 (100%)
==============================================================================

Hops                              AA%                time    amount     IBUs
==============================================================================
Northern Brewer (pellet)          9.9%             90 min   0.89 oz    28.36
Saaz (leaf)                       3.1%             30 min   0.71 oz     4.62
Hallertau (pellet)                3.8%             15 min   0.53 oz     3.02
==============================================================================
                                                            2.12 oz    36.00

Speculative apparent attenuation and resulting ABV
==============================================================================
  Str.    Att.  ABV         Str.    Att.  ABV         Str.    Att.  ABV       
 1.029    60%   5.6%       1.022    70%   6.6%       1.014    80%   7.7%      
 1.025    65%   6.1%       1.018    75%   7.1%       1.011    85%   8.2%      
==============================================================================

```

Example recipe
---
```
name:   Return to IPAnema
yeast:  WLP095
volume: 19.5l
boil:   20min

mashparams:
        temperature: 66degC

fermentables:
        anchor: [ strength, 16degP ]

        mash:
                Briess Pale:     rest
                Avangard Vienna:  18%
                flaked oats:      10%

        ferment:
                table sugar:       5%

hops:
        defs:
                mosaic:         [ Mosaic,     12.4%,   pellet  ]
                citra:          [ Citra,      14.0%,   pellet  ]

        boil:
                - [ citra,      10g, 20 min ]
                - [ mosaic,     10g, 20 min ]
                - [ citra,      10g,  2 min ]
                - [ mosaic,     10g,  2 min ]

        steep:
                - [ citra,      15g, 10min @ 95degC ]
                - [ mosaic,     15g, 10min @ 95degC ]

        dryhop:
                - [ citra,      11g, 3 day -> 0 day ]
                - [ mosaic,     11g, 3 day -> 0 day ]
                - [ citra,      52g, keg ]
                - [ mosaic,     52g, keg ]
```
translated with `wbctool -u metric -u plato`:
```
==============================================================================
Name:              Return to IPAnema
Final volume:      19.5l              Boil:                20 min             
IBU (Tinseth):     19.85              BUGU:                0.29               
Color (Morey):     11.7 EBC, 5.9 SRM  Water (20.0°C):      29.4l              
Pitch rate, ale:   256 billion        Pitch rate, lager:   513 billion        

Yeast:             WLP095
Water notes:       

Preboil  volume  : 23.1l (70.0°C)     Measured:                               
Preboil  strength: 13.9°P             Measured:                               
Postboil volume  : 22.3l (100.0°C)    Measured:                               
Postboil strength: 14.9°P             Measured:                               

Kettle loss (est): 1.0l               Fermenter loss (est):0.9l               
Mash eff (conf) :  88.0%              Brewhouse eff (est): 84.8%              

NOTE: keg hops absorb: 0.6l => effective yield: 18.9l
NOTE: keg hop volume: ~0.2l => packaged volume: 19.7l
==============================================================================

Fermentables                                  amount  ext (100%)   ext (88%)
==============================================================================
Mash
------------------------------------------------------------------------------
Briess Pale                         3.53 kg ( 67.0%)     2.77 kg     2.44 kg
Avangard Vienna                       948 g ( 18.0%)       714 g       629 g
flaked oats                           527 g ( 10.0%)       369 g       325 g
------------------------------------------------------------------------------
                                    5.01 kg ( 95.0%)     3.85 kg     3.39 kg
Ferment
------------------------------------------------------------------------------
table sugar                           263 g (  5.0%)       263 g       263 g
------------------------------------------------------------------------------
                                      263 g (  5.0%)       263 g       263 g
==============================================================================
                                    5.27 kg (100.0%)     4.12 kg     3.66 kg

Mashing instructions (for ambient temperature 20.0°C)
==============================================================================
66.0°C : add 12.9l of water at 78.5°C (2.50 l/kg, mash vol 16.0l)
Mashstep water volume: 12.5l @ 20.0°C
Sparge water volume:   17.4l @ 82.0°C
First wort (conv. %):  20.3°P (85%), 21.3°P (90%), 22.4°P (95%), 23.5°P (100%)
==============================================================================

Hops                              AA%                time    amount     IBUs
==============================================================================
Citra (pellet)                    14.0%            20 min    10.0 g     9.24
Mosaic (pellet)                   12.4%            20 min    10.0 g     8.18
Citra (pellet)                    14.0%             2 min    10.0 g     1.29
Mosaic (pellet)                   12.4%             2 min    10.0 g     1.14
------------------------------------------------------------------------------
Citra (pellet)                    14.0%    10min @ 95.0°C    15.0 g     0.00
Mosaic (pellet)                   12.4%    10min @ 95.0°C    15.0 g     0.00
------------------------------------------------------------------------------
Citra (pellet)                    14.0%     dryhop 3 => 0    11.0 g     0.00
Mosaic (pellet)                   12.4%     dryhop 3 => 0    11.0 g     0.00
Mosaic (pellet)                   12.4%     dryhop in keg    52.0 g     0.00
Citra (pellet)                    14.0%     dryhop in keg    52.0 g     0.00
==============================================================================
                                                              196 g    19.85

Speculative apparent attenuation and resulting ABV
==============================================================================
  Str.    Att.  ABV         Str.    Att.  ABV         Str.    Att.  ABV       
 6.9°P    60%   5.3%       5.2°P    70%   6.3%       3.5°P    80%   7.3%      
 6.1°P    65%   5.8%       4.4°P    75%   6.8%       2.6°P    85%   7.8%      
==============================================================================

```

translated with `wbctool -u us -u sg`:
```
==============================================================================
Name:              Return to IPAnema
Final volume:      5.2gal             Boil:                20 min             
IBU (Tinseth):     19.85              BUGU:                0.29               
Color (Morey):     11.7 EBC, 5.9 SRM  Water (68.0°F):      7.8gal             
Pitch rate, ale:   256 billion        Pitch rate, lager:   513 billion        

Yeast:             WLP095
Water notes:       

Preboil  volume  : 6.1gal (158.0°F)   Measured:                               
Preboil  strength: 1.056              Measured:                               
Postboil volume  : 5.9gal (212.0°F)   Measured:                               
Postboil strength: 1.061              Measured:                               

Kettle loss (est): 0.3gal             Fermenter loss (est):0.2gal             
Mash eff (conf) :  88.0%              Brewhouse eff (est): 84.8%              

NOTE: keg hops absorb: 0.2gal => effective yield: 5.0gal
NOTE: keg hop volume: ~0.1gal => packaged volume: 5.2gal
==============================================================================

Fermentables                                  amount  ext (100%)   ext (88%)
==============================================================================
Mash
------------------------------------------------------------------------------
Briess Pale                        7 3/4 lb ( 67.0%)   6 1/16 lb    5 3/8 lb
Avangard Vienna                   2 1/16 lb ( 18.0%)   1 9/16 lb    1 3/8 lb
flaked oats                        1 1/8 lb ( 10.0%)    13.01 oz    11.45 oz
------------------------------------------------------------------------------
                                    11 0 lb ( 95.0%)   8 7/16 lb   7 7/16 lb
Ferment
------------------------------------------------------------------------------
table sugar                         9.29 oz (  5.0%)     9.29 oz     9.29 oz
------------------------------------------------------------------------------
                                    9.29 oz (  5.0%)     9.29 oz     9.29 oz
==============================================================================
                                 11 9/16 lb (100.0%)   9 1/16 lb      8 0 lb

Mashing instructions (for ambient temperature 68.0°F)
==============================================================================
150.8°F: add 3.4gal of water at 173.3°F (1.20 qt/lb, mash vol 4.2gal)
Mashstep water volume: 3.3gal @ 68.0°F
Sparge water volume:   4.6gal @ 179.6°F
First wort (conv. %):  1.084 (85%), 1.089 (90%), 1.094 (95%), 1.099 (100%)
==============================================================================

Hops                              AA%                time    amount     IBUs
==============================================================================
Citra (pellet)                    14.0%            20 min   0.35 oz     9.24
Mosaic (pellet)                   12.4%            20 min   0.35 oz     8.18
Citra (pellet)                    14.0%             2 min   0.35 oz     1.29
Mosaic (pellet)                   12.4%             2 min   0.35 oz     1.14
------------------------------------------------------------------------------
Citra (pellet)                    14.0%   10min @ 203.0°F   0.53 oz     0.00
Mosaic (pellet)                   12.4%   10min @ 203.0°F   0.53 oz     0.00
------------------------------------------------------------------------------
Citra (pellet)                    14.0%     dryhop 3 => 0   0.39 oz     0.00
Mosaic (pellet)                   12.4%     dryhop 3 => 0   0.39 oz     0.00
Mosaic (pellet)                   12.4%     dryhop in keg   1.83 oz     0.00
Citra (pellet)                    14.0%     dryhop in keg   1.83 oz     0.00
==============================================================================
                                                            6.91 oz    19.85

Speculative apparent attenuation and resulting ABV
==============================================================================
  Str.    Att.  ABV         Str.    Att.  ABV         Str.    Att.  ABV       
 1.027    60%   5.3%       1.021    70%   6.3%       1.014    80%   7.3%      
 1.024    65%   5.8%       1.017    75%   6.8%       1.010    85%   7.8%      
==============================================================================

```

