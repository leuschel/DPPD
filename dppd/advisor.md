DPPD: advisor

The "advisor" Benchmark
-----------------------

Part of the [DPPD Library](../dppd.html).

### General Description

This is a benchmark developed by Thomas Horvath. It can be fully unfolded and contains no built-in's nor negations.

### The benchmark program

what\_to\_do\_today( \_today, \_weather, \_program ):-
    kind\_of\_day( \_today, \_daykind ),
    kind\_of\_weather( \_weather, \_weatherkind ),
    proposal( \_daykind, \_weatherkind, _program ).

kind\_of\_day( monday, workday ). 
kind\_of\_day( thuesday, workday ).
kind\_of\_day( wednesday, workday ).
kind\_of\_day( thursday, workday ).
kind\_of\_day( friday, workday ).
kind\_of\_day( saturday, weekend ).
kind\_of\_day( sunday, weekend ).
kind\_of\_day( eastern, feastday ).
kind\_of\_day( first\_of\_may, feastday ).
kind\_of\_day( christmas, feastday ).
kind\_of\_day( new\_years\_day, badday ).
kind\_of\_day( friday\_the\_13th, badday ).

kind\_of\_weather( sunny, nice ).
kind\_of\_weather( rainy, nasty ).
kind\_of\_weather( foggy, nasty ).
kind\_of\_weather( windy, nasty ).

proposal( workday, _, go\_to\_work ).
proposal( weekend, nice, go\_out\_to\_the\_nature ).
proposal( weekend, nice, visit\_the\_golf_club ).
proposal( weekend, nice, wash\_your\_car ).
proposal( weekend, nasty, go\_out\_to\_the\_town ).
proposal( weekend, nasty, visit\_the\_bridge_club ).
proposal( weekend, nasty, enjoy\_yourself\_at_home ).
proposal( weekend, _, it\_is\_fun\_to\_learn_Japanese ).
proposal( badday, _, you\_had\_better\_stay\_in_bed ).
proposal( feastday, \_weather, \_program ) :-
    proposal( weekend, \_weather, \_program ).

### The partial deduction query

 :\- what\_to\_do\_today( first\_of\_may, \_weather, _program ).

### The renamed run-time queries

 :\- what\_to\_do\_today\_\_1( sunny, _program ).
 :\- what\_to\_do\_today\_\_1( \_wheather, enjoy\_yourself\_at\_home ).
 :\- what\_to\_do\_today\_\_1( foggy, _program ).
 :\- what\_to\_do\_today\_\_1( \_wheather, wash\_your_car ).
 :\- what\_to\_do\_today\_\_1(  nice, wash\_your\_car )

### Example solution

what\_to\_do\_today\_\_1(sunny,go\_out\_to\_the\_nature).
what\_to\_do\_today\_\_1(sunny,visit\_the\_golf_club).
what\_to\_do\_today\_\_1(sunny,wash\_your\_car).
what\_to\_do\_today\_\_1(sunny,it\_is\_fun\_to\_learn_Japanese).
what\_to\_do\_today\_\_1(rainy,go\_out\_to\_the\_town).
what\_to\_do\_today\_\_1(rainy,visit\_the\_bridge_club).
what\_to\_do\_today\_\_1(rainy,enjoy\_yourself\_at_home).
what\_to\_do\_today\_\_1(rainy,it\_is\_fun\_to\_learn_Japanese).
what\_to\_do\_today\_\_1(foggy,go\_out\_to\_the\_town).
what\_to\_do\_today\_\_1(foggy,visit\_the\_bridge_club).
what\_to\_do\_today\_\_1(foggy,enjoy\_yourself\_at_home).
what\_to\_do\_today\_\_1(foggy,it\_is\_fun\_to\_learn_Japanese).
what\_to\_do\_today\_\_1(windy,go\_out\_to\_the\_town).
what\_to\_do\_today\_\_1(windy,visit\_the\_bridge_club).
what\_to\_do\_today\_\_1(windy,enjoy\_yourself\_at_home).
what\_to\_do\_today\_\_1(windy,it\_is\_fun\_to\_learn_Japanese).

* * *

[Imprint](http://www.stups.uni-duesseldorf.de/w/Imprint)