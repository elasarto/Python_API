
Weather Analysis - Observed Trends:

- Cities closest to the equater have higher maximum temperatures. Cities farther away from the equator have lower maximum temperatures.
- Humidity increases as latitude approaches the equator.
- Cloudiness decreases as latitude approaches the equator.
- The majority of cities in the data set have wind speeds from 0 - 20mph; speeds appear to decrease as latitude approaches the equator. 





```python
#dependencies
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import requests
import json
from citipy import citipy

#api_key
from config import api_key
```


```python
# Save config information.
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "Imperial"

query_url = "%sappid=%s&units=%s&q="%(url, api_key, units)

```


```python
#find random coordinates
lats = np.random.randint(0,91, size=(2000)) #northern hemisphere values only
lngs = np.random.randint(-180,181, size=(2000))
```


```python
#zip coordinates 
coordinates = list(zip(lats, lngs))
coordinates
```




    [(4, -143),
     (11, 40),
     (61, 126),
     (25, 163),
     (60, 96),
     (45, -143),
     (11, -62),
     (86, 26),
     (84, 165),
     (13, 25),
     (68, -68),
     (6, 35),
     (9, 141),
     (34, 103),
     (9, -82),
     (50, 169),
     (9, -110),
     (16, -40),
     (8, -52),
     (32, -98),
     (5, -73),
     (62, -154),
     (54, -61),
     (29, 41),
     (48, 49),
     (28, -29),
     (20, 10),
     (16, 15),
     (68, -45),
     (90, -92),
     (52, 122),
     (77, -122),
     (54, 44),
     (15, -104),
     (3, 12),
     (32, -157),
     (55, -126),
     (25, -61),
     (55, 82),
     (6, -134),
     (65, -141),
     (61, 82),
     (8, -136),
     (86, -8),
     (46, -156),
     (78, -103),
     (52, -114),
     (71, -147),
     (67, 149),
     (64, 56),
     (10, -40),
     (46, 114),
     (78, -86),
     (56, -115),
     (24, -107),
     (72, -51),
     (43, -103),
     (82, 33),
     (48, -96),
     (69, 64),
     (84, 120),
     (57, 61),
     (21, 43),
     (6, 147),
     (88, 79),
     (14, 36),
     (7, -142),
     (87, -112),
     (80, -105),
     (6, 159),
     (35, 160),
     (84, 113),
     (34, 64),
     (42, -20),
     (26, -14),
     (77, 10),
     (10, 9),
     (84, 103),
     (46, -22),
     (58, 123),
     (26, -60),
     (24, -121),
     (0, -8),
     (28, -174),
     (79, 149),
     (51, -10),
     (49, 0),
     (13, -178),
     (50, -99),
     (30, -112),
     (47, 37),
     (12, -74),
     (49, 134),
     (90, 49),
     (42, 104),
     (76, 42),
     (77, 135),
     (77, -96),
     (33, 46),
     (77, 138),
     (68, -62),
     (16, 95),
     (67, -85),
     (11, 14),
     (72, 125),
     (13, -175),
     (41, 96),
     (24, 158),
     (4, -17),
     (49, -72),
     (19, -28),
     (25, -148),
     (17, -149),
     (28, 52),
     (27, 175),
     (2, -103),
     (16, 135),
     (1, -50),
     (36, 131),
     (87, -168),
     (29, 43),
     (61, 101),
     (12, 16),
     (20, 98),
     (50, -119),
     (87, -130),
     (63, 8),
     (20, 16),
     (57, -166),
     (55, 21),
     (71, 16),
     (49, 1),
     (74, -25),
     (59, -144),
     (77, 158),
     (5, -144),
     (82, 173),
     (21, -42),
     (42, 82),
     (47, 111),
     (49, -48),
     (81, 89),
     (10, 50),
     (55, 32),
     (6, 57),
     (83, 108),
     (52, 165),
     (38, -134),
     (77, 165),
     (41, 166),
     (61, 128),
     (18, 162),
     (60, -95),
     (55, -166),
     (17, -70),
     (47, 62),
     (79, -148),
     (42, 130),
     (81, 82),
     (5, 175),
     (61, 10),
     (55, 142),
     (6, 96),
     (12, 76),
     (77, -124),
     (20, -133),
     (77, -8),
     (61, 112),
     (61, 105),
     (72, -100),
     (11, 133),
     (62, -69),
     (53, -5),
     (90, -2),
     (47, 110),
     (22, 99),
     (33, 84),
     (23, 54),
     (89, 14),
     (72, 109),
     (34, -153),
     (30, -60),
     (29, 127),
     (10, -170),
     (8, 9),
     (20, 74),
     (83, -33),
     (83, -86),
     (73, -1),
     (48, 63),
     (64, -114),
     (82, -152),
     (45, 149),
     (65, -20),
     (2, 127),
     (90, 10),
     (67, -2),
     (90, -18),
     (23, -131),
     (84, 122),
     (56, -168),
     (49, 125),
     (33, -169),
     (28, -55),
     (68, 107),
     (75, -146),
     (31, -44),
     (21, -30),
     (81, -34),
     (79, -92),
     (68, 72),
     (35, -117),
     (42, 11),
     (70, -164),
     (89, 154),
     (79, -56),
     (86, -42),
     (51, -165),
     (71, 173),
     (47, 125),
     (68, -127),
     (4, -85),
     (79, -111),
     (31, 134),
     (1, -66),
     (46, 144),
     (24, 69),
     (38, 44),
     (62, -144),
     (81, 76),
     (25, 179),
     (79, -104),
     (86, -12),
     (28, -140),
     (47, 101),
     (20, 94),
     (29, 76),
     (83, -178),
     (81, -116),
     (57, -175),
     (44, -51),
     (10, -56),
     (13, -85),
     (16, 179),
     (75, 141),
     (13, 164),
     (67, 98),
     (55, -16),
     (52, 56),
     (42, -83),
     (85, -16),
     (37, -173),
     (71, 142),
     (17, 105),
     (0, 147),
     (43, -18),
     (47, 42),
     (7, -84),
     (40, 170),
     (64, -13),
     (29, -127),
     (10, 118),
     (24, 103),
     (86, 171),
     (84, -180),
     (34, -174),
     (77, -105),
     (46, -93),
     (29, -149),
     (79, 61),
     (69, -148),
     (37, 98),
     (9, -122),
     (22, 132),
     (29, 85),
     (1, 106),
     (32, 110),
     (71, -106),
     (23, -42),
     (15, 163),
     (50, 118),
     (66, 96),
     (32, -52),
     (18, -101),
     (3, 84),
     (49, -116),
     (24, 5),
     (3, 114),
     (60, 169),
     (63, -70),
     (42, 131),
     (79, -178),
     (49, -2),
     (68, 177),
     (58, -169),
     (66, 108),
     (25, 4),
     (36, 62),
     (9, 111),
     (32, -34),
     (84, 72),
     (82, -27),
     (87, 135),
     (78, -13),
     (36, 165),
     (51, -112),
     (64, -49),
     (3, 35),
     (71, 45),
     (69, 17),
     (21, -159),
     (8, 107),
     (73, -58),
     (84, 99),
     (44, -120),
     (70, -48),
     (43, -107),
     (34, 10),
     (57, 94),
     (24, 74),
     (22, 55),
     (75, 66),
     (80, 153),
     (62, 142),
     (70, 67),
     (84, -155),
     (77, 59),
     (67, 31),
     (83, -102),
     (72, 13),
     (56, -94),
     (56, 31),
     (24, 29),
     (13, 37),
     (74, 84),
     (63, -92),
     (64, -170),
     (53, 95),
     (69, 23),
     (36, 109),
     (2, -44),
     (43, -39),
     (50, -177),
     (89, -168),
     (79, -142),
     (70, -120),
     (76, -16),
     (87, 167),
     (73, -163),
     (80, -157),
     (21, -20),
     (33, -123),
     (84, -173),
     (44, 155),
     (12, 114),
     (46, 12),
     (29, 37),
     (40, 107),
     (4, -136),
     (49, -163),
     (37, 132),
     (60, 143),
     (17, -150),
     (76, 172),
     (38, 32),
     (75, -164),
     (81, -98),
     (64, -127),
     (1, 136),
     (2, -38),
     (15, 111),
     (59, -52),
     (25, 98),
     (57, 161),
     (3, 76),
     (31, -14),
     (13, -95),
     (50, 1),
     (14, -8),
     (37, 128),
     (33, 75),
     (61, -149),
     (57, -76),
     (83, -104),
     (90, 165),
     (55, 120),
     (65, -170),
     (49, 40),
     (47, -160),
     (20, 0),
     (83, -158),
     (39, -3),
     (67, 104),
     (70, 162),
     (24, -43),
     (14, -53),
     (21, -171),
     (9, 97),
     (78, 118),
     (48, -122),
     (67, -128),
     (55, -9),
     (84, -37),
     (31, 113),
     (84, -11),
     (66, -98),
     (55, -113),
     (74, 147),
     (30, -14),
     (59, -51),
     (50, -85),
     (80, -173),
     (69, -141),
     (75, 31),
     (51, -136),
     (80, -79),
     (64, 10),
     (87, -162),
     (28, -15),
     (73, -125),
     (64, -63),
     (13, -54),
     (81, 10),
     (31, -30),
     (63, -45),
     (90, -45),
     (5, 120),
     (41, 178),
     (86, 49),
     (15, 16),
     (32, 97),
     (28, -119),
     (67, 105),
     (89, 156),
     (87, -144),
     (88, -28),
     (60, 55),
     (51, -44),
     (82, -106),
     (69, 148),
     (6, -90),
     (69, -30),
     (19, -59),
     (79, -174),
     (60, -32),
     (33, 4),
     (10, 150),
     (78, -119),
     (27, 110),
     (56, -108),
     (24, -30),
     (24, -150),
     (10, -172),
     (54, 35),
     (17, 24),
     (11, 121),
     (31, 109),
     (31, -156),
     (76, 98),
     (44, 120),
     (4, 147),
     (43, -68),
     (50, 44),
     (39, 175),
     (2, 80),
     (75, -65),
     (81, -29),
     (32, -13),
     (24, -20),
     (43, 105),
     (35, -180),
     (69, -43),
     (2, 65),
     (84, -75),
     (51, -34),
     (87, -146),
     (39, -46),
     (33, 139),
     (89, -131),
     (70, -19),
     (37, 149),
     (1, 72),
     (19, -114),
     (83, 4),
     (76, 107),
     (83, -135),
     (53, 78),
     (15, -123),
     (83, -27),
     (30, -150),
     (35, -85),
     (64, 26),
     (78, -32),
     (79, 18),
     (52, 106),
     (86, 3),
     (12, -134),
     (79, -18),
     (56, -160),
     (30, -30),
     (2, 22),
     (27, -161),
     (78, 55),
     (87, -108),
     (65, 130),
     (46, 109),
     (18, 12),
     (86, 177),
     (39, -120),
     (88, -142),
     (50, -164),
     (74, -174),
     (64, -63),
     (4, 46),
     (13, -11),
     (72, 147),
     (39, 146),
     (85, -161),
     (67, -128),
     (68, 127),
     (2, -116),
     (0, 174),
     (32, -177),
     (76, -138),
     (18, 70),
     (73, -136),
     (41, -50),
     (20, 141),
     (71, -39),
     (22, 84),
     (22, 51),
     (84, 154),
     (90, 73),
     (44, -145),
     (77, 10),
     (52, 93),
     (33, 179),
     (60, 65),
     (19, -110),
     (57, -39),
     (47, 136),
     (34, -71),
     (30, -86),
     (21, -11),
     (14, -95),
     (85, -104),
     (46, 141),
     (53, -43),
     (35, 148),
     (3, 117),
     (86, -99),
     (64, 17),
     (53, 172),
     (11, 2),
     (20, 160),
     (45, -124),
     (30, -58),
     (0, -22),
     (13, 7),
     (24, 173),
     (83, 57),
     (49, -119),
     (61, -106),
     (3, 3),
     (56, 176),
     (83, 116),
     (77, 65),
     (63, -112),
     (53, 90),
     (79, 15),
     (38, 177),
     (12, -95),
     (76, 9),
     (82, -90),
     (1, -34),
     (13, 123),
     (54, 90),
     (37, 171),
     (15, -75),
     (24, -52),
     (1, -29),
     (9, -146),
     (56, 135),
     (18, -141),
     (39, 36),
     (58, -86),
     (22, -155),
     (88, 133),
     (40, 98),
     (89, -51),
     (86, -60),
     (83, -27),
     (9, -9),
     (85, 134),
     (76, -63),
     (28, -14),
     (52, -107),
     (88, -85),
     (13, 120),
     (31, -26),
     (39, 83),
     (74, -179),
     (1, -116),
     (32, 139),
     (70, 118),
     (69, -62),
     (0, 67),
     (41, -143),
     (41, 84),
     (65, -142),
     (1, 145),
     (50, 4),
     (60, 67),
     (41, 83),
     (38, 177),
     (90, -76),
     (14, -6),
     (1, 110),
     (60, -8),
     (43, 4),
     (32, -98),
     (48, -56),
     (42, 29),
     (11, -41),
     (54, -83),
     (23, -169),
     (23, -78),
     (78, -92),
     (44, 27),
     (66, 86),
     (3, 158),
     (61, 72),
     (5, -39),
     (23, 26),
     (22, -8),
     (79, 169),
     (40, 101),
     (78, 143),
     (7, -103),
     (56, 96),
     (28, 108),
     (88, 7),
     (45, -159),
     (47, 101),
     (50, 67),
     (53, -180),
     (50, -59),
     (50, -140),
     (83, 87),
     (17, 33),
     (40, 7),
     (81, -149),
     (48, 141),
     (28, 101),
     (52, -25),
     (1, -144),
     (72, 37),
     (56, -129),
     (70, -93),
     (5, -93),
     (56, -26),
     (54, -175),
     (18, -87),
     (2, 176),
     (65, -92),
     (75, 156),
     (43, -100),
     (23, 144),
     (20, 59),
     (75, 75),
     (62, 37),
     (42, 97),
     (49, -159),
     (44, -164),
     (87, 28),
     (29, 63),
     (59, -15),
     (51, 92),
     (29, -17),
     (37, 115),
     (71, -98),
     (55, 11),
     (16, -78),
     (26, -85),
     (56, -126),
     (12, 101),
     (84, 136),
     (67, -175),
     (30, -58),
     (56, -21),
     (67, -145),
     (44, 63),
     (41, 124),
     (14, 123),
     (34, 37),
     (65, 175),
     (11, -123),
     (82, -89),
     (0, -75),
     (56, 169),
     (56, -118),
     (35, 91),
     (6, 104),
     (84, -126),
     (67, 140),
     (38, 19),
     (86, 50),
     (20, -27),
     (65, 85),
     (26, 80),
     (90, -122),
     (67, -91),
     (27, 6),
     (56, -132),
     (41, 10),
     (53, 123),
     (56, 31),
     (12, -4),
     (24, 61),
     (0, 62),
     (45, -68),
     (56, -131),
     (42, 22),
     (5, -168),
     (18, 128),
     (10, -111),
     (16, -178),
     (17, 98),
     (13, 10),
     (55, -15),
     (17, -130),
     (80, -50),
     (31, 15),
     (73, 95),
     (35, -150),
     (36, -116),
     (8, -8),
     (47, 18),
     (36, 178),
     (79, 71),
     (77, 122),
     (11, -85),
     (8, 79),
     (10, -93),
     (28, -179),
     (13, -154),
     (85, -95),
     (81, -92),
     (30, -149),
     (78, 24),
     (82, 74),
     (6, -24),
     (35, -148),
     (69, 117),
     (17, -93),
     (67, -137),
     (41, 13),
     (72, 75),
     (14, -16),
     (53, -74),
     (80, -168),
     (12, -167),
     (65, 164),
     (61, 157),
     (72, 57),
     (75, 101),
     (51, -28),
     (46, 161),
     (84, 53),
     (59, 5),
     (77, -158),
     (26, 26),
     (77, -166),
     (84, 74),
     (62, 80),
     (39, 151),
     (70, 84),
     (27, -101),
     (39, 115),
     (73, -140),
     (21, 144),
     (60, -152),
     (24, -84),
     (80, 131),
     (28, 93),
     (54, -111),
     (22, -14),
     (14, 120),
     (45, 161),
     (28, -176),
     (54, 50),
     (10, -112),
     (41, -141),
     (33, 29),
     (27, 55),
     (71, 142),
     (69, -115),
     (71, 152),
     (33, -163),
     (0, -34),
     (24, -69),
     (24, -112),
     (43, 132),
     (33, -164),
     (78, -70),
     (49, 29),
     (53, -35),
     (1, 120),
     (42, -139),
     (27, 6),
     (57, -157),
     (62, -21),
     (71, -80),
     (39, -126),
     (73, 115),
     (1, 52),
     (33, -9),
     (75, 94),
     (61, 69),
     (28, -114),
     (9, 120),
     (15, -172),
     (45, -84),
     (68, 81),
     (48, 116),
     (83, 35),
     (36, -70),
     (80, -51),
     (2, 170),
     (43, -85),
     (39, 143),
     (40, -156),
     (42, -58),
     (22, -88),
     (5, 80),
     (38, -72),
     (40, 154),
     (39, -177),
     (80, 99),
     (53, -100),
     (23, -42),
     (36, 133),
     (63, 42),
     (56, -165),
     (50, -146),
     (49, 8),
     (81, 165),
     (2, 82),
     (1, -11),
     (42, -130),
     (86, 103),
     (47, -119),
     (62, -108),
     (23, 97),
     (66, -144),
     (62, 32),
     (51, -73),
     (61, -9),
     (70, -10),
     (60, 101),
     (71, 50),
     (17, -179),
     (7, -160),
     (25, -112),
     (45, -143),
     (44, -39),
     (76, -7),
     (65, -37),
     (21, -59),
     (3, 179),
     (48, 159),
     (89, -66),
     (60, -124),
     (70, -87),
     (70, 25),
     (88, -74),
     (77, 52),
     (65, -110),
     (31, 171),
     (21, -44),
     (67, 39),
     (54, -168),
     (55, -143),
     (87, 34),
     (63, 113),
     (73, 44),
     (21, 52),
     (44, 25),
     (14, -127),
     (75, -6),
     (44, -57),
     (73, -53),
     (63, 37),
     (31, -110),
     (45, -21),
     (22, -57),
     (72, 96),
     (39, 32),
     (59, -166),
     (17, -126),
     (26, -76),
     (54, 34),
     (37, -99),
     (22, 120),
     (70, 24),
     (48, 166),
     (81, 112),
     (20, -75),
     (49, 94),
     (16, 123),
     (54, -139),
     (46, -63),
     (46, -58),
     (58, 96),
     (80, -141),
     (0, 89),
     (2, 121),
     (30, 101),
     (31, 29),
     (45, -136),
     (64, -130),
     (22, 162),
     (5, 77),
     (65, 20),
     (25, -68),
     (29, 172),
     (43, 103),
     (8, -173),
     (56, -145),
     (84, -147),
     (49, 167),
     (69, 144),
     (26, -17),
     (10, 115),
     (44, -7),
     (73, 168),
     (68, -90),
     (53, 57),
     (73, -37),
     (58, -123),
     (2, 84),
     (36, -29),
     (3, -40),
     (50, 86),
     (36, -132),
     (1, 53),
     (59, -75),
     (0, 154),
     (28, -20),
     (86, 165),
     (90, -135),
     (23, 141),
     (43, -108),
     (10, 110),
     (50, -1),
     (54, -67),
     (76, -126),
     (67, -11),
     (13, 89),
     (14, 11),
     (17, -130),
     (29, -24),
     (86, 43),
     (78, -83),
     (17, 36),
     (4, 83),
     (41, -152),
     (31, -164),
     (61, -172),
     (53, -3),
     (21, -167),
     (39, -50),
     (13, 19),
     (14, 175),
     (1, 31),
     (79, 1),
     (79, 89),
     (54, 99),
     (23, 34),
     (39, 99),
     (33, -13),
     (41, -20),
     (11, -159),
     (62, 5),
     (82, 124),
     (32, -23),
     (76, -101),
     (33, 85),
     (33, 50),
     (73, -78),
     (62, 26),
     (51, 5),
     (56, 33),
     (81, -72),
     (59, 169),
     (55, -14),
     (60, -30),
     (11, -20),
     (43, -28),
     (67, 100),
     ...]




```python
cities = []
for coordinate_pair in coordinates:
    lat, lon = coordinate_pair
    cities.append(citipy.nearest_city(lat, lon))
```


```python
names = []
codes = []

for city in cities:
    country_code = city.country_code
    codes.append(country_code)
    
    name = city.city_name
    names.append(name)
```


```python
len(names)
```




    2000




```python
#zip cities 
zip_cities = list(zip(names, codes))
len(zip_cities)
```




    2000




```python
unique_names = []

for zip_city in zip_cities:
    if zip_city not in unique_names:
        unique_names.append(zip_city)

```


```python
len(unique_names)
```




    887




```python
unique_df = pd.DataFrame(unique_names, columns=['City Name', 'Country Code'])
unique_df["Latitude"] = ""
unique_df["Longitude"] = ""
unique_df["Temperature (F)"] = ""
unique_df["Humidity"] = ""
unique_df["Cloudiness"] = ""
unique_df["Wind Speed"] = ""
unique_df["Date"] = ""
unique_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City Name</th>
      <th>Country Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature (F)</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
      <th>Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>atuona</td>
      <td>pf</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>bati</td>
      <td>et</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>berdigestyakh</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>butaritari</td>
      <td>ki</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>baykit</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




```python
len(unique_names)
```




    887




```python

# Loop through the list of cities and perform a request for data on each
for index, unique_name in enumerate(unique_names):
    name, code = unique_name
    response = requests.get(query_url + name, code).json()
    #print(json.dumps(response, indent=4, sort_keys=True))
    try:
        temp = (response['main']['temp_max'])
        humidity = (response['main']['humidity'])
        clouds = (response['clouds']['all'])
        wind = (response['wind']['speed'])
        latitude = (response['coord']['lat'])
        longitude = (response['coord']['lon'])
        date = (response['dt']) 
            
        print(query_url + name + code)
        print(f"Retrieving Results for City #" + str(index) + ": " + str(name).title() + "," + str(code).upper())
    
        unique_df.set_value(index, "Latitude", latitude)
        unique_df.set_value(index, "Longitude", longitude)
        unique_df.set_value(index, "Temperature (F)", temp)
        unique_df.set_value(index, "Humidity", humidity)
        unique_df.set_value(index, "Cloudiness", clouds)
        unique_df.set_value(index, "Wind Speed", wind)
        unique_df.set_value(index, "Date", date)
    
    except(KeyError, IndexError):
        print(f"Error Retrieving Results for City #" + str(index) + ": " + str(name).title() + ", " + str(code).upper() + "." + " Skipping record.")
        continue    
    
print(f"------------------------")        
print(f"Data Retrieval Complete.")        
print(f"------------------------")

unique_df.head()
```

    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=atuonapf
    Retrieving Results for City #0: Atuona,PF
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=batiet
    Retrieving Results for City #1: Bati,ET
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=berdigestyakhru
    Retrieving Results for City #2: Berdigestyakh,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=butaritariki
    Retrieving Results for City #3: Butaritari,KI
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=baykitru
    Retrieving Results for City #4: Baykit,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sitkaus
    Retrieving Results for City #5: Sitka,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mucurapott
    Retrieving Results for City #6: Mucurapo,TT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=longyearbyensj
    Retrieving Results for City #7: Longyearbyen,SJ
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pevekru
    Retrieving Results for City #8: Pevek,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kutumsd
    Retrieving Results for City #9: Kutum,SD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=clyde riverca
    Retrieving Results for City #10: Clyde River,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mizan teferiet
    Retrieving Results for City #11: Mizan Teferi,ET
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=airaipw
    Retrieving Results for City #12: Airai,PW
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=linxiacn
    Retrieving Results for City #13: Linxia,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=chiriqui grandepa
    Retrieving Results for City #14: Chiriqui Grande,PA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nikolskoyeru
    Retrieving Results for City #15: Nikolskoye,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=san patriciomx
    Retrieving Results for City #16: San Patricio,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ponta do solcv
    Retrieving Results for City #17: Ponta Do Sol,CV
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sinnamarygf
    Retrieving Results for City #18: Sinnamary,GF
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=stephenvilleus
    Retrieving Results for City #19: Stephenville,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=campohermosoco
    Retrieving Results for City #20: Campohermoso,CO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kenaius
    Retrieving Results for City #21: Kenai,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=saint-augustinca
    Retrieving Results for City #22: Saint-Augustin,CA
    Error Retrieving Results for City #23: Sakakah, SA. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=aksarayskiyru
    Retrieving Results for City #24: Aksarayskiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ponta delgadapt
    Retrieving Results for City #25: Ponta Delgada,PT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=arlitne
    Retrieving Results for City #26: Arlit,NE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=maotd
    Retrieving Results for City #27: Mao,TD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ilulissatgl
    Retrieving Results for City #28: Ilulissat,GL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=qaanaaqgl
    Retrieving Results for City #29: Qaanaaq,GL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=genhecn
    Retrieving Results for City #30: Genhe,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=norman wellsca
    Retrieving Results for City #31: Norman Wells,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kovylkinoru
    Retrieving Results for City #32: Kovylkino,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lazaro cardenasmx
    Retrieving Results for City #33: Lazaro Cardenas,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sangmelimacm
    Retrieving Results for City #34: Sangmelima,CM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kapaaus
    Retrieving Results for City #35: Kapaa,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=burns lakeca
    Retrieving Results for City #36: Burns Lake,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=the valleyai
    Retrieving Results for City #37: The Valley,AI
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=chikru
    Retrieving Results for City #38: Chik,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mayoca
    Retrieving Results for City #39: Mayo,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kargasokru
    Retrieving Results for City #40: Kargasok,RU
    Error Retrieving Results for City #41: Illoqqortoormiut, GL. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kodiakus
    Retrieving Results for City #42: Kodiak,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yellowknifeca
    Retrieving Results for City #43: Yellowknife,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=penholdca
    Retrieving Results for City #44: Penhold,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=collegeus
    Retrieving Results for City #45: College,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=zyryankaru
    Retrieving Results for City #46: Zyryanka,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nizhniy odesru
    Retrieving Results for City #47: Nizhniy Odes,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=carutaperabr
    Retrieving Results for City #48: Carutapera,BR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=baruun-urtmn
    Retrieving Results for City #49: Baruun-Urt,MN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=slave lakeca
    Retrieving Results for City #50: Slave Lake,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=el carrizomx
    Retrieving Results for City #51: El Carrizo,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=rapid valleyus
    Retrieving Results for City #52: Rapid Valley,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=vardono
    Retrieving Results for City #53: Vardo,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=grand forksus
    Retrieving Results for City #54: Grand Forks,US
    Error Retrieving Results for City #55: Severnyy, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=saskylakhru
    Retrieving Results for City #56: Saskylakh,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=staropyshminskru
    Retrieving Results for City #57: Staropyshminsk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=abhasa
    Retrieving Results for City #58: Abha,SA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lorengaupg
    Retrieving Results for City #59: Lorengau,PG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=diksonru
    Retrieving Results for City #60: Dikson,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dokasd
    Retrieving Results for City #61: Doka,SD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=namatanaipg
    Retrieving Results for City #62: Namatanai,PG
    Error Retrieving Results for City #63: Sentyabrskiy, RU. Skipping record.
    Error Retrieving Results for City #64: Sahrak, AF. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=vila franca do campopt
    Retrieving Results for City #65: Vila Franca Do Campo,PT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=aguimeses
    Retrieving Results for City #66: Aguimes,ES
    Error Retrieving Results for City #67: Barentsburg, SJ. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=josng
    Retrieving Results for City #68: Jos,NG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=khatangaru
    Retrieving Results for City #69: Khatanga,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=praia da vitoriapt
    Retrieving Results for City #70: Praia Da Vitoria,PT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=neryungriru
    Retrieving Results for City #71: Neryungri,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hamiltonbm
    Retrieving Results for City #72: Hamilton,BM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=guerrero negromx
    Retrieving Results for City #73: Guerrero Negro,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=harperlr
    Retrieving Results for City #74: Harper,LR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=chokurdakhru
    Retrieving Results for City #75: Chokurdakh,RU
    Error Retrieving Results for City #76: Bantry, IE. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=argentanfr
    Retrieving Results for City #77: Argentan,FR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=carberryca
    Retrieving Results for City #78: Carberry,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pitiquitomx
    Retrieving Results for City #79: Pitiquito,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=osypenkoua
    Retrieving Results for City #80: Osypenko,UA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=santa martaco
    Retrieving Results for City #81: Santa Marta,CO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=smidovichru
    Retrieving Results for City #82: Smidovich,RU
    Error Retrieving Results for City #83: Belushya Guba, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hovdmn
    Retrieving Results for City #84: Hovd,MN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ostrovnoyru
    Retrieving Results for City #85: Ostrovnoy,RU
    Error Retrieving Results for City #86: Nizhneyansk, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=thompsonca
    Retrieving Results for City #87: Thompson,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mehranir
    Retrieving Results for City #88: Mehran,IR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pangnirtungca
    Retrieving Results for City #89: Pangnirtung,CA
    Error Retrieving Results for City #90: Labutta, MM. Skipping record.
    Error Retrieving Results for City #91: Attawapiskat, CA. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=moracm
    Retrieving Results for City #92: Mora,CM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tiksiru
    Retrieving Results for City #93: Tiksi,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yumencn
    Retrieving Results for City #94: Yumen,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hasakijp
    Retrieving Results for City #95: Hasaki,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bonthesl
    Retrieving Results for City #96: Bonthe,SL
    Error Retrieving Results for City #97: Dolbeau, CA. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hilous
    Retrieving Results for City #98: Hilo,US
    Error Retrieving Results for City #99: Khormuj, IR. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=puerto ayoraec
    Retrieving Results for City #100: Puerto Ayora,EC
    Error Retrieving Results for City #101: Tucuma, BR. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hamadajp
    Retrieving Results for City #102: Hamada,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=barrowus
    Retrieving Results for City #103: Barrow,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=buraydahsa
    Retrieving Results for City #104: Buraydah,SA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=vanavararu
    Retrieving Results for City #105: Vanavara,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dourbalitd
    Retrieving Results for City #106: Dourbali,TD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mae hong sonth
    Retrieving Results for City #107: Mae Hong Son,TH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lumbyca
    Retrieving Results for City #108: Lumby,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tuktoyaktukca
    Retrieving Results for City #109: Tuktoyaktuk,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=rensvikno
    Retrieving Results for City #110: Rensvik,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bilmane
    Retrieving Results for City #111: Bilma,NE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bethelus
    Retrieving Results for City #112: Bethel,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=polesskru
    Retrieving Results for City #113: Polessk,RU
    Error Retrieving Results for City #114: Andenes, NO. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=evreuxfr
    Retrieving Results for City #115: Evreux,FR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=palmerus
    Retrieving Results for City #116: Palmer,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=cherskiyru
    Retrieving Results for City #117: Cherskiy,RU
    Error Retrieving Results for City #118: Kuche, CN. Skipping record.
    Error Retrieving Results for City #119: Ondorhaan, MN. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=torbayca
    Retrieving Results for City #120: Torbay,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bandarbeylaso
    Retrieving Results for City #121: Bandarbeyla,SO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=smolenskru
    Retrieving Results for City #122: Smolensk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=fortunaus
    Retrieving Results for City #123: Fortuna,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=severo-kurilskru
    Retrieving Results for City #124: Severo-Kurilsk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mokhsogollokhru
    Retrieving Results for City #125: Mokhsogollokh,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=banido
    Retrieving Results for City #126: Bani,DO
    Error Retrieving Results for City #127: Kazalinsk, KZ. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=khasanru
    Retrieving Results for City #128: Khasan,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dokkano
    Retrieving Results for City #129: Dokka,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=okharu
    Retrieving Results for City #130: Okha,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sigliid
    Retrieving Results for City #131: Sigli,ID
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ponnampetin
    Retrieving Results for City #132: Ponnampet,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lompocus
    Retrieving Results for City #133: Lompoc,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=husavikis
    Retrieving Results for City #134: Husavik,IS
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=peleduyru
    Retrieving Results for City #135: Peleduy,RU
    Error Retrieving Results for City #136: Meyungs, PW. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=iqaluitca
    Retrieving Results for City #137: Iqaluit,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=llangefnigb
    Retrieving Results for City #138: Llangefni,GB
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=darhanmn
    Retrieving Results for City #139: Darhan,MN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lashiomm
    Retrieving Results for City #140: Lashio,MM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=jumlanp
    Retrieving Results for City #141: Jumla,NP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=abu dhabiae
    Retrieving Results for City #142: Abu Dhabi,AE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ahuimanuus
    Retrieving Results for City #143: Ahuimanu,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=saint georgebm
    Retrieving Results for City #144: Saint George,BM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nazejp
    Retrieving Results for City #145: Naze,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=makurding
    Retrieving Results for City #146: Makurdi,NG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ojharin
    Retrieving Results for City #147: Ojhar,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=klaksvikfo
    Retrieving Results for City #148: Klaksvik,FO
    Error Retrieving Results for City #149: Blonduos, IS. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ternateid
    Retrieving Results for City #150: Ternate,ID
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nenjiangcn
    Retrieving Results for City #151: Nenjiang,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=aykhalru
    Retrieving Results for City #152: Aykhal,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ribeira grandept
    Retrieving Results for City #153: Ribeira Grande,PT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tasiilaqgl
    Retrieving Results for City #154: Tasiilaq,GL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yar-saleru
    Retrieving Results for City #155: Yar-Sale,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=barstowus
    Retrieving Results for City #156: Barstow,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=orbetelloit
    Retrieving Results for City #157: Orbetello,IT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nomeus
    Retrieving Results for City #158: Nome,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=upernavikgl
    Retrieving Results for City #159: Upernavik,GL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=komsomolskiyru
    Retrieving Results for City #160: Komsomolskiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ranghulucn
    Retrieving Results for City #161: Ranghulu,CN
    Error Retrieving Results for City #162: Burica, PA. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nakamurajp
    Retrieving Results for City #163: Nakamura,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sao gabriel da cachoeirabr
    Retrieving Results for City #164: Sao Gabriel Da Cachoeira,BR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=novikovoru
    Retrieving Results for City #165: Novikovo,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kadhanpk
    Retrieving Results for City #166: Kadhan,PK
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hakkaritr
    Retrieving Results for City #167: Hakkari,TR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=fairbanksus
    Retrieving Results for City #168: Fairbanks,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=minbumm
    Retrieving Results for City #169: Minbu,MM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hansiin
    Retrieving Results for City #170: Hansi,IN
    Error Retrieving Results for City #171: Mys Shmidta, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=provideniyaru
    Retrieving Results for City #172: Provideniya,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=georgetowngy
    Retrieving Results for City #173: Georgetown,GY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=rio blanconi
    Retrieving Results for City #174: Rio Blanco,NI
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=deputatskiyru
    Retrieving Results for City #175: Deputatskiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=turaru
    Retrieving Results for City #176: Tura,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dingleie
    Retrieving Results for City #177: Dingle,IE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=matveyevkaru
    Retrieving Results for City #178: Matveyevka,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=colchesterca
    Retrieving Results for City #179: Colchester,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=that phanomth
    Retrieving Results for City #180: That Phanom,TH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=muroses
    Retrieving Results for City #181: Muros,ES
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=orlovskiyru
    Retrieving Results for City #182: Orlovskiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hofnis
    Retrieving Results for City #183: Hofn,IS
    Error Retrieving Results for City #184: Aporawan, PH. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kaiyuancn
    Retrieving Results for City #185: Kaiyuan,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=north branchus
    Retrieving Results for City #186: North Branch,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kahuluius
    Retrieving Results for City #187: Kahului,US
    Error Retrieving Results for City #188: Amderma, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=jiuquancn
    Retrieving Results for City #189: Jiuquan,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=cabo san lucasmx
    Retrieving Results for City #190: Cabo San Lucas,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nishiharajp
    Retrieving Results for City #191: Nishihara,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pokharanp
    Retrieving Results for City #192: Pokhara,NP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kijangid
    Retrieving Results for City #193: Kijang,ID
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=shiyancn
    Retrieving Results for City #194: Shiyan,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=krasnokamenskru
    Retrieving Results for City #195: Krasnokamensk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=zirandaromx
    Retrieving Results for City #196: Zirandaro,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hambantotalk
    Retrieving Results for City #197: Hambantota,LK
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=crestonca
    Retrieving Results for City #198: Creston,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=gatly
    Retrieving Results for City #199: Gat,LY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bintulumy
    Retrieving Results for City #200: Bintulu,MY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tilichikiru
    Retrieving Results for City #201: Tilichiki,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=saint-malofr
    Retrieving Results for City #202: Saint-Malo,FR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=leningradskiyru
    Retrieving Results for City #203: Leningradskiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=adrardz
    Retrieving Results for City #204: Adrar,DZ
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sarakhsir
    Retrieving Results for City #205: Sarakhs,IR
    Error Retrieving Results for City #206: Phan Rang, VN. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=brooksca
    Retrieving Results for City #207: Brooks,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=paamiutgl
    Retrieving Results for City #208: Paamiut,GL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=morotoug
    Retrieving Results for City #209: Moroto,UG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kamenkaru
    Retrieving Results for City #210: Kamenka,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=harstadno
    Retrieving Results for City #211: Harstad,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=makahaus
    Retrieving Results for City #212: Makaha,US
    Error Retrieving Results for City #213: Bac Lieu, VN. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=redmondus
    Retrieving Results for City #214: Redmond,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=casperus
    Retrieving Results for City #215: Casper,US
    Error Retrieving Results for City #216: Qabis, TN. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=predivinskru
    Retrieving Results for City #217: Predivinsk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=salumbarin
    Retrieving Results for City #218: Salumbar,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ust-neraru
    Retrieving Results for City #219: Ust-Nera,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=labytnangiru
    Retrieving Results for City #220: Labytnangi,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=alakurttiru
    Retrieving Results for City #221: Alakurtti,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=gravdalno
    Retrieving Results for City #222: Gravdal,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kunyaru
    Retrieving Results for City #223: Kunya,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sohageg
    Retrieving Results for City #224: Sohag,EG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=gondaret
    Retrieving Results for City #225: Gondar,ET
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lavrentiyaru
    Retrieving Results for City #226: Lavrentiya,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=toora-khemru
    Retrieving Results for City #227: Toora-Khem,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kautokeinono
    Retrieving Results for City #228: Kautokeino,NO
    Error Retrieving Results for City #229: Yanan, CN. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=aklavikca
    Retrieving Results for City #230: Aklavik,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nouadhiboumr
    Retrieving Results for City #231: Nouadhibou,MR
    Error Retrieving Results for City #232: Candawaga, PH. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=valdobbiadeneit
    Retrieving Results for City #233: Valdobbiadene,IT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tabuksa
    Retrieving Results for City #234: Tabuk,SA
    Error Retrieving Results for City #235: Haibowan, CN. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=izumojp
    Retrieving Results for City #236: Izumo,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=okhotskru
    Retrieving Results for City #237: Okhotsk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=beysehirtr
    Retrieving Results for City #238: Beysehir,TR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=biakid
    Retrieving Results for City #239: Biak,ID
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=itaremabr
    Retrieving Results for City #240: Itarema,BR
    Error Retrieving Results for City #241: Qui Nhon, VN. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=myitkyinamm
    Retrieving Results for City #242: Myitkyina,MM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=klyuchiru
    Retrieving Results for City #243: Klyuchi,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mulimv
    Retrieving Results for City #244: Muli,MV
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=teguisees
    Retrieving Results for City #245: Teguise,ES
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pochutlamx
    Retrieving Results for City #246: Pochutla,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dieppefr
    Retrieving Results for City #247: Dieppe,FR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kolokaniml
    Retrieving Results for City #248: Kolokani,ML
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=seoulkr
    Retrieving Results for City #249: Seoul,KR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=katrain
    Retrieving Results for City #250: Katra,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=chapaisca
    Retrieving Results for City #251: Chapais,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tupikru
    Retrieving Results for City #252: Tupik,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=krasnovkaru
    Retrieving Results for City #253: Krasnovka,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tessalitml
    Retrieving Results for City #254: Tessalit,ML
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tomellosoes
    Retrieving Results for City #255: Tomelloso,ES
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bathshebabb
    Retrieving Results for City #256: Bathsheba,BB
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=phangngath
    Retrieving Results for City #257: Phangnga,TH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=west lake stevensus
    Retrieving Results for City #258: West Lake Stevens,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=killybegsie
    Retrieving Results for City #259: Killybegs,IE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tianmencn
    Retrieving Results for City #260: Tianmen,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=athabascaca
    Retrieving Results for City #261: Athabasca,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hornepayneca
    Retrieving Results for City #262: Hornepayne,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=batsfjordno
    Retrieving Results for City #263: Batsfjord,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=oldenno
    Retrieving Results for City #264: Olden,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=teldees
    Retrieving Results for City #265: Telde,ES
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=qaqortoqgl
    Retrieving Results for City #266: Qaqortoq,GL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=paranganph
    Retrieving Results for City #267: Parangan,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tezuin
    Retrieving Results for City #268: Tezu,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=san quintinmx
    Retrieving Results for City #269: San Quintin,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kosaru
    Retrieving Results for City #270: Kosa,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nanortalikgl
    Retrieving Results for City #271: Nanortalik,GL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=santa cruzcr
    Retrieving Results for City #272: Santa Cruz,CR
    Error Retrieving Results for City #273: Bolungarvik, IS. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=codringtonag
    Retrieving Results for City #274: Codrington,AG
    Error Retrieving Results for City #275: Warqla, DZ. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hongjiangcn
    Retrieving Results for City #276: Hongjiang,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=meadow lakeca
    Retrieving Results for City #277: Meadow Lake,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=duminichiru
    Retrieving Results for City #278: Duminichi,RU
    Error Retrieving Results for City #279: Cuyo, PH. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=wanxiancn
    Retrieving Results for City #280: Wanxian,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=chifengcn
    Retrieving Results for City #281: Chifeng,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bar harborus
    Retrieving Results for City #282: Bar Harbor,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=danilovkaru
    Retrieving Results for City #283: Danilovka,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mataralk
    Retrieving Results for City #284: Matara,LK
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=narsaqgl
    Retrieving Results for City #285: Narsaq,GL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mandalgovimn
    Retrieving Results for City #286: Mandalgovi,MN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kudahuvadhoomv
    Retrieving Results for City #287: Kudahuvadhoo,MV
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lagoapt
    Retrieving Results for City #288: Lagoa,PT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=shimodajp
    Retrieving Results for City #289: Shimoda,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nemurojp
    Retrieving Results for City #290: Nemuro,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=thinadhoomv
    Retrieving Results for City #291: Thinadhoo,MV
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=burlaru
    Retrieving Results for City #292: Burla,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=constitucionmx
    Retrieving Results for City #293: Constitucion,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=east brainerdus
    Retrieving Results for City #294: East Brainerd,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=muhosfi
    Retrieving Results for City #295: Muhos,FI
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=istokru
    Retrieving Results for City #296: Istok,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bumbacd
    Retrieving Results for City #297: Bumba,CD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=namtsyru
    Retrieving Results for City #298: Namtsy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=south lake tahoeus
    Retrieving Results for City #299: South Lake Tahoe,US
    Error Retrieving Results for City #300: Mahadday Weyne, SO. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=direml
    Retrieving Results for City #301: Dire,ML
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yamadajp
    Retrieving Results for City #302: Yamada,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=batagay-alytaru
    Retrieving Results for City #303: Batagay-Alyta,RU
    Error Retrieving Results for City #304: Temaraia, KI. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=diuin
    Retrieving Results for City #305: Diu,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sundargarhin
    Retrieving Results for City #306: Sundargarh,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=abu samrahqa
    Retrieving Results for City #307: Abu Samrah,QA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=shagonarru
    Retrieving Results for City #308: Shagonar,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=urayru
    Retrieving Results for City #309: Uray,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=vostokru
    Retrieving Results for City #310: Vostok,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=elizabeth cityus
    Retrieving Results for City #311: Elizabeth City,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=panama cityus
    Retrieving Results for City #312: Panama City,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=atarmr
    Retrieving Results for City #313: Atar,MR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=salina cruzmx
    Retrieving Results for City #314: Salina Cruz,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=wakkanaijp
    Retrieving Results for City #315: Wakkanai,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tarakanid
    Retrieving Results for City #316: Tarakan,ID
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=timrase
    Retrieving Results for City #317: Timra,SE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=banikoarabj
    Retrieving Results for City #318: Banikoara,BJ
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=newportus
    Retrieving Results for City #319: Newport,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=georgetownsh
    Retrieving Results for City #320: Georgetown,SH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=madarounfane
    Retrieving Results for City #321: Madarounfa,NE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=osoyoosca
    Retrieving Results for City #322: Osoyoos,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=la rongeca
    Retrieving Results for City #323: La Ronge,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=cotonoubj
    Retrieving Results for City #324: Cotonou,BJ
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=beringovskiyru
    Retrieving Results for City #325: Beringovskiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tashtypru
    Retrieving Results for City #326: Tashtyp,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=puerto maderomx
    Retrieving Results for City #327: Puerto Madero,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tourosbr
    Retrieving Results for City #328: Touros,BR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=san rafaelph
    Retrieving Results for City #329: San Rafael,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sorskru
    Retrieving Results for City #330: Sorsk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=morant bayjm
    Retrieving Results for City #331: Morant Bay,JM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=chumikanru
    Retrieving Results for City #332: Chumikan,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=talastr
    Retrieving Results for City #333: Talas,TR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kerouanegn
    Retrieving Results for City #334: Kerouane,GN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=puerto del rosarioes
    Retrieving Results for City #335: Puerto Del Rosario,ES
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=saskatoonca
    Retrieving Results for City #336: Saskatoon,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=paluanph
    Retrieving Results for City #337: Paluan,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=fourmiesfr
    Retrieving Results for City #338: Fourmies,FR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kondinskoyeru
    Retrieving Results for City #339: Kondinskoye,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nionoml
    Retrieving Results for City #340: Niono,ML
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kuchingmy
    Retrieving Results for City #341: Kuching,MY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=vagurfo
    Retrieving Results for City #342: Vagur,FO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=setefr
    Retrieving Results for City #343: Sete,FR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=harbour bretonca
    Retrieving Results for City #344: Harbour Breton,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=durusutr
    Retrieving Results for City #345: Durusu,TR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=cayennegf
    Retrieving Results for City #346: Cayenne,GF
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=moroncu
    Retrieving Results for City #347: Moron,CU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ciocanestiro
    Retrieving Results for City #348: Ciocanesti,RO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=igarkaru
    Retrieving Results for City #349: Igarka,RU
    Error Retrieving Results for City #350: Cheuskiny, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=asyuteg
    Retrieving Results for City #351: Asyut,EG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=taoudenniml
    Retrieving Results for City #352: Taoudenni,ML
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=zhangyecn
    Retrieving Results for City #353: Zhangye,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=acapulcomx
    Retrieving Results for City #354: Acapulco,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ilanskiyru
    Retrieving Results for City #355: Ilanskiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tongzicn
    Retrieving Results for City #356: Tongzi,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=derzhavinskkz
    Retrieving Results for City #357: Derzhavinsk,KZ
    Error Retrieving Results for City #358: Barbar, SD. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=algheroit
    Retrieving Results for City #359: Alghero,IT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tomariru
    Retrieving Results for City #360: Tomari,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=xichangcn
    Retrieving Results for City #361: Xichang,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=grindavikis
    Retrieving Results for City #362: Grindavik,IS
    Error Retrieving Results for City #363: Tumannyy, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=terraceca
    Retrieving Results for City #364: Terrace,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=san pedrobz
    Retrieving Results for City #365: San Pedro,BZ
    Error Retrieving Results for City #366: Rawannawi, KI. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=srednekolymskru
    Retrieving Results for City #367: Srednekolymsk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pierreus
    Retrieving Results for City #368: Pierre,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tateyamajp
    Retrieving Results for City #369: Tateyama,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=surom
    Retrieving Results for City #370: Sur,OM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pudozhru
    Retrieving Results for City #371: Pudozh,RU
    Error Retrieving Results for City #372: Rudbar, AF. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=khandagaytyru
    Retrieving Results for City #373: Khandagayty,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=icod de los vinoses
    Retrieving Results for City #374: Icod De Los Vinos,ES
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tangshancn
    Retrieving Results for City #375: Tangshan,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nakskovdk
    Retrieving Results for City #376: Nakskov,DK
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bull savannajm
    Retrieving Results for City #377: Bull Savanna,JM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=veniceus
    Retrieving Results for City #378: Venice,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=smithersca
    Retrieving Results for City #379: Smithers,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sattahipth
    Retrieving Results for City #380: Sattahip,TH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=vestmannaeyjaris
    Retrieving Results for City #381: Vestmannaeyjar,IS
    Error Retrieving Results for City #382: Dzhusaly, KZ. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=xiaoshicn
    Retrieving Results for City #383: Xiaoshi,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=san felipeph
    Retrieving Results for City #384: San Felipe,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yabrudsy
    Retrieving Results for City #385: Yabrud,SY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=anadyrru
    Retrieving Results for City #386: Anadyr,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=puerto leguizamoco
    Retrieving Results for City #387: Puerto Leguizamo,CO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=fairviewca
    Retrieving Results for City #388: Fairview,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lasacn
    Retrieving Results for City #389: Lasa,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kuala terengganumy
    Retrieving Results for City #390: Kuala Terengganu,MY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lixouriongr
    Retrieving Results for City #391: Lixourion,GR
    Error Retrieving Results for City #392: Krasnoselkup, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kurarain
    Retrieving Results for City #393: Kurara,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ketchikanus
    Retrieving Results for City #394: Ketchikan,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=olbiait
    Retrieving Results for City #395: Olbia,IT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=urusharu
    Retrieving Results for City #396: Urusha,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=solenzobf
    Retrieving Results for City #397: Solenzo,BF
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=jiwanipk
    Retrieving Results for City #398: Jiwani,PK
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=victoriasc
    Retrieving Results for City #399: Victoria,SC
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ellsworthus
    Retrieving Results for City #400: Ellsworth,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sveti nikolemk
    Retrieving Results for City #401: Sveti Nikole,MK
    Error Retrieving Results for City #402: Saleaula, WS. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pandanph
    Retrieving Results for City #403: Pandan,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mae ramatth
    Retrieving Results for City #404: Mae Ramat,TH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ngurung
    Retrieving Results for City #405: Nguru,NG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bani walidly
    Retrieving Results for City #406: Bani Walid,LY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=talnakhru
    Retrieving Results for City #407: Talnakh,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pahrumpus
    Retrieving Results for City #408: Pahrump,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=toubaci
    Retrieving Results for City #409: Touba,CI
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=balatonalmadihu
    Retrieving Results for City #410: Balatonalmadi,HU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=upalacr
    Retrieving Results for City #411: Upala,CR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tiruchchendurin
    Retrieving Results for City #412: Tiruchchendur,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=champericogt
    Retrieving Results for City #413: Champerico,GT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=havoysundno
    Retrieving Results for City #414: Havoysund,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sao filipecv
    Retrieving Results for City #415: Sao Filipe,CV
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bochilmx
    Retrieving Results for City #416: Bochil,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sabaudiait
    Retrieving Results for City #417: Sabaudia,IT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kahonesn
    Retrieving Results for City #418: Kahone,SN
    Error Retrieving Results for City #419: Kamenskoye, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=omsukchanru
    Retrieving Results for City #420: Omsukchan,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kopervikno
    Retrieving Results for City #421: Kopervik,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=abnubeg
    Retrieving Results for City #422: Abnub,EG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=strezhevoyru
    Retrieving Results for City #423: Strezhevoy,RU
    Error Retrieving Results for City #424: Karaul, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=monclovamx
    Retrieving Results for City #425: Monclova,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dingzhoucn
    Retrieving Results for City #426: Dingzhou,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=homerus
    Retrieving Results for City #427: Homer,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bahia hondacu
    Retrieving Results for City #428: Bahia Honda,CU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ziroin
    Retrieving Results for City #429: Ziro,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=elk pointca
    Retrieving Results for City #430: Elk Point,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=cabraph
    Retrieving Results for City #431: Cabra,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=novaya malyklaru
    Retrieving Results for City #432: Novaya Malykla,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=port hardyca
    Retrieving Results for City #433: Port Hardy,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=alexandriaeg
    Retrieving Results for City #434: Alexandria,EG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bandar-e lengehir
    Retrieving Results for City #435: Bandar-E Lengeh,IR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=cockburn towntc
    Retrieving Results for City #436: Cockburn Town,TC
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=russkiyru
    Retrieving Results for City #437: Russkiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nemyrivua
    Retrieving Results for City #438: Nemyriv,UA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=paluid
    Retrieving Results for City #439: Palu,ID
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hobyoso
    Retrieving Results for City #440: Hobyo,SO
    Error Retrieving Results for City #441: Azimur, MA. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=gornopravdinskru
    Retrieving Results for City #442: Gornopravdinsk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=caramayph
    Retrieving Results for City #443: Caramay,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=alpenaus
    Retrieving Results for City #444: Alpena,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tazovskiyru
    Retrieving Results for City #445: Tazovskiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=zabaykalskru
    Retrieving Results for City #446: Zabaykalsk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nantucketus
    Retrieving Results for City #447: Nantucket,US
    Error Retrieving Results for City #448: Buariki, KI. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ioniaus
    Retrieving Results for City #449: Ionia,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kamaishijp
    Retrieving Results for City #450: Kamaishi,JP
    Error Retrieving Results for City #451: Louisbourg, CA. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=panabamx
    Retrieving Results for City #452: Panaba,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=weligamalk
    Retrieving Results for City #453: Weligama,LK
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=brigantineus
    Retrieving Results for City #454: Brigantine,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=the pasca
    Retrieving Results for City #455: The Pas,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sakaiminatojp
    Retrieving Results for City #456: Sakaiminato,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bereznikru
    Retrieving Results for City #457: Bereznik,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=landaude
    Retrieving Results for City #458: Landau,DE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=coos bayus
    Retrieving Results for City #459: Coos Bay,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=moses lakeus
    Retrieving Results for City #460: Moses Lake,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mogokmm
    Retrieving Results for City #461: Mogok,MM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=suoyarviru
    Retrieving Results for City #462: Suoyarvi,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=albanelca
    Retrieving Results for City #463: Albanel,CA
    Error Retrieving Results for City #464: Sorvag, FO. Skipping record.
    Error Retrieving Results for City #465: Rungata, KI. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=fort nelsonca
    Retrieving Results for City #466: Fort Nelson,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lakselvno
    Retrieving Results for City #467: Lakselv,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=berlevagno
    Retrieving Results for City #468: Berlevag,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=chernyshevskiyru
    Retrieving Results for City #469: Chernyshevskiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=troianulro
    Retrieving Results for City #470: Troianul,RO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=saint-pierrepm
    Retrieving Results for City #471: Saint-Pierre,PM
    Error Retrieving Results for City #472: Maloshuyka, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=cananeamx
    Retrieving Results for City #473: Cananea,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=emirdagtr
    Retrieving Results for City #474: Emirdag,TR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dunmore townbs
    Retrieving Results for City #475: Dunmore Town,BS
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=betlitsaru
    Retrieving Results for City #476: Betlitsa,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=woodwardus
    Retrieving Results for City #477: Woodward,US
    Error Retrieving Results for City #478: Tungkang, TW. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=rypefjordno
    Retrieving Results for City #479: Rypefjord,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=guantanamocu
    Retrieving Results for City #480: Guantanamo,CU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=erzinru
    Retrieving Results for City #481: Erzin,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dicabisaganph
    Retrieving Results for City #482: Dicabisagan,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=charlottetownca
    Retrieving Results for City #483: Charlottetown,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=burgeoca
    Retrieving Results for City #484: Burgeo,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=motyginoru
    Retrieving Results for City #485: Motygino,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=meulabohid
    Retrieving Results for City #486: Meulaboh,ID
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=gorontaloid
    Retrieving Results for City #487: Gorontalo,ID
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yaancn
    Retrieving Results for City #488: Yaan,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kanniyakumariin
    Retrieving Results for City #489: Kanniyakumari,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=skelleftease
    Retrieving Results for City #490: Skelleftea,SE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=belaya goraru
    Retrieving Results for City #491: Belaya Gora,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=aronaes
    Retrieving Results for City #492: Arona,ES
    Error Retrieving Results for City #493: Taburi, PH. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=fozes
    Retrieving Results for City #494: Foz,ES
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=starosubkhangulovoru
    Retrieving Results for City #495: Starosubkhangulovo,RU
    Error Retrieving Results for City #496: Acarau, BR. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ust-koksaru
    Retrieving Results for City #497: Ust-Koksa,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=los llanos de aridanees
    Retrieving Results for City #498: Los Llanos De Aridane,ES
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=rivertonus
    Retrieving Results for City #499: Riverton,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tourlavillefr
    Retrieving Results for City #500: Tourlaville,FR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sept-ilesca
    Retrieving Results for City #501: Sept-Iles,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=port blairin
    Retrieving Results for City #502: Port Blair,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=gourene
    Retrieving Results for City #503: Goure,NE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=wagarsd
    Retrieving Results for City #504: Wagar,SD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=wrexhamgb
    Retrieving Results for City #505: Wrexham,GB
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=atitd
    Retrieving Results for City #506: Ati,TD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kagadiug
    Retrieving Results for City #507: Kagadi,UG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=shumskiyru
    Retrieving Results for City #508: Shumskiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=aswaneg
    Retrieving Results for City #509: Aswan,EG
    Error Retrieving Results for City #510: Porto Santo, PT. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=maloyno
    Retrieving Results for City #511: Maloy,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ponta do solpt
    Retrieving Results for City #512: Ponta Do Sol,PT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=fereydunshahrir
    Retrieving Results for City #513: Fereydunshahr,IR
    Error Retrieving Results for City #514: Jyvaskyla, FI. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=diestbe
    Retrieving Results for City #515: Diest,BE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=belyyru
    Retrieving Results for City #516: Belyy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=westportie
    Retrieving Results for City #517: Westport,IE
    Error Retrieving Results for City #518: Olafsvik, IS. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=oussouyesn
    Retrieving Results for City #519: Oussouye,SN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kholtosonru
    Retrieving Results for City #520: Kholtoson,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=fukuejp
    Retrieving Results for City #521: Fukue,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lawrenceburgus
    Retrieving Results for City #522: Lawrenceburg,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pacific groveus
    Retrieving Results for City #523: Pacific Grove,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dzhebariki-khayaru
    Retrieving Results for City #524: Dzhebariki-Khaya,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=visnesno
    Retrieving Results for City #525: Visnes,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=puerto escondidomx
    Retrieving Results for City #526: Puerto Escondido,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=prince rupertca
    Retrieving Results for City #527: Prince Rupert,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=aksaraytr
    Retrieving Results for City #528: Aksaray,TR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=macapabr
    Retrieving Results for City #529: Macapa,BR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yulitw
    Retrieving Results for City #530: Yuli,TW
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=grand-santigf
    Retrieving Results for City #531: Grand-Santi,GF
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=indianolaus
    Retrieving Results for City #532: Indianola,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=rochegdaru
    Retrieving Results for City #533: Rochegda,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=abu kamalsy
    Retrieving Results for City #534: Abu Kamal,SY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dukung
    Retrieving Results for City #535: Duku,NG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=clervauxlu
    Retrieving Results for City #536: Clervaux,LU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=moose factoryca
    Retrieving Results for City #537: Moose Factory,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ciudad bolivarve
    Retrieving Results for City #538: Ciudad Bolivar,VE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bidang
    Retrieving Results for City #539: Bida,NG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=zhiganskru
    Retrieving Results for City #540: Zhigansk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mahasamundin
    Retrieving Results for City #541: Mahasamund,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bushehrir
    Retrieving Results for City #542: Bushehr,IR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=inuvikca
    Retrieving Results for City #543: Inuvik,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=teyaru
    Retrieving Results for City #544: Teya,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kirunase
    Retrieving Results for City #545: Kiruna,SE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dickinsonus
    Retrieving Results for City #546: Dickinson,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=naifarumv
    Retrieving Results for City #547: Naifaru,MV
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=evenskru
    Retrieving Results for City #548: Evensk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yondoco
    Retrieving Results for City #549: Yondo,CO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=armanru
    Retrieving Results for City #550: Arman,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=jeremieht
    Retrieving Results for City #551: Jeremie,HT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tigilru
    Retrieving Results for City #552: Tigil,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=jaleswarin
    Retrieving Results for City #553: Jaleswar,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=petropavlovsk-kamchatskiyru
    Retrieving Results for City #554: Petropavlovsk-Kamchatskiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=riyadhsa
    Retrieving Results for City #555: Riyadh,SA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=akcakocatr
    Retrieving Results for City #556: Akcakoca,TR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mahibadhoomv
    Retrieving Results for City #557: Mahibadhoo,MV
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=port-gentilga
    Retrieving Results for City #558: Port-Gentil,GA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=chandilin
    Retrieving Results for City #559: Chandil,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nieuw nickeriesr
    Retrieving Results for City #560: Nieuw Nickerie,SR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sennoby
    Retrieving Results for City #561: Senno,BY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=takoradigh
    Retrieving Results for City #562: Takoradi,GH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=turanru
    Retrieving Results for City #563: Turan,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=progresomx
    Retrieving Results for City #564: Progreso,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bitamga
    Retrieving Results for City #565: Bitam,GA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=coahuayanamx
    Retrieving Results for City #566: Coahuayana,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=georgetownus
    Retrieving Results for City #567: Georgetown,US
    Error Retrieving Results for City #568: Omutinskoye, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=belyy yarru
    Retrieving Results for City #569: Belyy Yar,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=berlinus
    Retrieving Results for City #570: Berlin,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bitungid
    Retrieving Results for City #571: Bitung,ID
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kalevalaru
    Retrieving Results for City #572: Kalevala,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=artesiaus
    Retrieving Results for City #573: Artesia,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sisimiutgl
    Retrieving Results for City #574: Sisimiut,GL
    Error Retrieving Results for City #575: Asfi, MA. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=diffane
    Retrieving Results for City #576: Diffa,NE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=moronmn
    Retrieving Results for City #577: Moron,MN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kaviengpg
    Retrieving Results for City #578: Kavieng,PG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=middle islandkn
    Retrieving Results for City #579: Middle Island,KN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=maine-soroane
    Retrieving Results for City #580: Maine-Soroa,NE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pelymru
    Retrieving Results for City #581: Pelym,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=alice townbs
    Retrieving Results for City #582: Alice Town,BS
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mapiripanco
    Retrieving Results for City #583: Mapiripan,CO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=penichept
    Retrieving Results for City #584: Peniche,PT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=washimin
    Retrieving Results for City #585: Washim,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=makhachkalaru
    Retrieving Results for City #586: Makhachkala,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=havre-saint-pierreca
    Retrieving Results for City #587: Havre-Saint-Pierre,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=subaph
    Retrieving Results for City #588: Suba,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bubaquegw
    Retrieving Results for City #589: Bubaque,GW
    Error Retrieving Results for City #590: Buqayq, SA. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kangaatsiaqgl
    Retrieving Results for City #591: Kangaatsiaq,GL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kalmunailk
    Retrieving Results for City #592: Kalmunai,LK
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=isla vistaus
    Retrieving Results for City #593: Isla Vista,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=gagnoaci
    Retrieving Results for City #594: Gagnoa,CI
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=shangzhicn
    Retrieving Results for City #595: Shangzhi,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=rhedennl
    Retrieving Results for City #596: Rheden,NL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=khorqa
    Retrieving Results for City #597: Khor,QA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=camalumx
    Retrieving Results for City #598: Camalu,MX
    Error Retrieving Results for City #599: Tabukiniberu, KI. Skipping record.
    Error Retrieving Results for City #600: Quebo, GW. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yeniseyskru
    Retrieving Results for City #601: Yeniseysk,RU
    Error Retrieving Results for City #602: Stornoway, GB. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=gazanjyktm
    Retrieving Results for City #603: Gazanjyk,TM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=turukhanskru
    Retrieving Results for City #604: Turukhansk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=manadoid
    Retrieving Results for City #605: Manado,ID
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=haines junctionca
    Retrieving Results for City #606: Haines Junction,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=severo-yeniseyskiyru
    Retrieving Results for City #607: Severo-Yeniseyskiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=libengecd
    Retrieving Results for City #608: Libenge,CD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=san jeronimomx
    Retrieving Results for City #609: San Jeronimo,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=talayaru
    Retrieving Results for City #610: Talaya,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=umm kaddadahsd
    Retrieving Results for City #611: Umm Kaddadah,SD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kavarattiin
    Retrieving Results for City #612: Kavaratti,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ordutr
    Retrieving Results for City #613: Ordu,TR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=makakilo cityus
    Retrieving Results for City #614: Makakilo City,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kietapg
    Retrieving Results for City #615: Kieta,PG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=daoukroci
    Retrieving Results for City #616: Daoukro,CI
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=uclueletca
    Retrieving Results for City #617: Ucluelet,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=vanimopg
    Retrieving Results for City #618: Vanimo,PG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=simbahanph
    Retrieving Results for City #619: Simbahan,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sobolevoru
    Retrieving Results for City #620: Sobolevo,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lake cityus
    Retrieving Results for City #621: Lake City,US
    Error Retrieving Results for City #622: Shchelyayur, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=cascaispt
    Retrieving Results for City #623: Cascais,PT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bhasawarin
    Retrieving Results for City #624: Bhasawar,IN
    Error Retrieving Results for City #625: Talesh, IR. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tielicn
    Retrieving Results for City #626: Tieli,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=matayeg
    Retrieving Results for City #627: Matay,EG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=fronteramx
    Retrieving Results for City #628: Frontera,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=maniitsoqgl
    Retrieving Results for City #629: Maniitsoq,GL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=beidaocn
    Retrieving Results for City #630: Beidao,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=managf
    Retrieving Results for City #631: Mana,GF
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nyurbaru
    Retrieving Results for City #632: Nyurba,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=barahonado
    Retrieving Results for City #633: Barahona,DO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=washougalus
    Retrieving Results for City #634: Washougal,US
    Error Retrieving Results for City #635: Safaga, EG. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=severomuyskru
    Retrieving Results for City #636: Severomuysk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hayesvilleus
    Retrieving Results for City #637: Hayesville,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=laguna de perlasni
    Retrieving Results for City #638: Laguna De Perlas,NI
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bragancapt
    Retrieving Results for City #639: Braganca,PT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=marienburgsr
    Retrieving Results for City #640: Marienburg,SR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=waddanly
    Retrieving Results for City #641: Waddan,LY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=okoneshnikovoru
    Retrieving Results for City #642: Okoneshnikovo,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nioroml
    Retrieving Results for City #643: Nioro,ML
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dukatru
    Retrieving Results for City #644: Dukat,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mulegemx
    Retrieving Results for City #645: Mulege,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=buchananlr
    Retrieving Results for City #646: Buchanan,LR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ixtapamx
    Retrieving Results for City #647: Ixtapa,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=salymru
    Retrieving Results for City #648: Salym,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yelovoru
    Retrieving Results for City #649: Yelovo,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=chisecgt
    Retrieving Results for City #650: Chisec,GT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=paradise valleyus
    Retrieving Results for City #651: Paradise Valley,US
    Error Retrieving Results for City #652: Alappuzha, IN. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=clarence townbs
    Retrieving Results for City #653: Clarence Town,BS
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lagosng
    Retrieving Results for City #654: Lagos,NG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bilibinoru
    Retrieving Results for City #655: Bilibino,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=la banezaes
    Retrieving Results for City #656: La Baneza,ES
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=qazvinir
    Retrieving Results for City #657: Qazvin,IR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=faanuipf
    Retrieving Results for City #658: Faanui,PF
    Error Retrieving Results for City #659: Qibili, TN. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=alamosmx
    Retrieving Results for City #660: Alamos,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nnewing
    Retrieving Results for City #661: Nnewi,NG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tazmaltdz
    Retrieving Results for City #662: Tazmalt,DZ
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=coriaes
    Retrieving Results for City #663: Coria,ES
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=warrenus
    Retrieving Results for City #664: Warren,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kaunaslt
    Retrieving Results for City #665: Kaunas,LT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=texarkanaus
    Retrieving Results for City #666: Texarkana,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=saint anthonyca
    Retrieving Results for City #667: Saint Anthony,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tymovskoyeru
    Retrieving Results for City #668: Tymovskoye,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=canchungogw
    Retrieving Results for City #669: Canchungo,GW
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=triel-sur-seinefr
    Retrieving Results for City #670: Triel-Sur-Seine,FR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=santa ritave
    Retrieving Results for City #671: Santa Rita,VE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=shilkaru
    Retrieving Results for City #672: Shilka,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=carlsbadus
    Retrieving Results for City #673: Carlsbad,US
    Error Retrieving Results for City #674: Glubokoe, KZ. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=fort abbaspk
    Retrieving Results for City #675: Fort Abbas,PK
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=manderake
    Retrieving Results for City #676: Mandera,KE
    Error Retrieving Results for City #677: Fort Saint John, CA. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kizhingaru
    Retrieving Results for City #678: Kizhinga,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tayoltitamx
    Retrieving Results for City #679: Tayoltita,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=castanheira de perapt
    Retrieving Results for City #680: Castanheira De Pera,PT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=san policarpoph
    Retrieving Results for City #681: San Policarpo,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=jacmelht
    Retrieving Results for City #682: Jacmel,HT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sontrade
    Retrieving Results for City #683: Sontra,DE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=matamorosmx
    Retrieving Results for City #684: Matamoros,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=port huenemeus
    Retrieving Results for City #685: Port Hueneme,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kragujevacrs
    Retrieving Results for City #686: Kragujevac,RS
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=linfencn
    Retrieving Results for City #687: Linfen,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ousru
    Retrieving Results for City #688: Ous,RU
    Error Retrieving Results for City #689: Ajoloapan, MX. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=praiacv
    Retrieving Results for City #690: Praia,CV
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sawakinsd
    Retrieving Results for City #691: Sawakin,SD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bosasoso
    Retrieving Results for City #692: Bosaso,SO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=halmeuro
    Retrieving Results for City #693: Halmeu,RO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kapitmy
    Retrieving Results for City #694: Kapit,MY
    Error Retrieving Results for City #695: Ciras, AF. Skipping record.
    Error Retrieving Results for City #696: Odemis, TR. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=manavalakurichiin
    Retrieving Results for City #697: Manavalakurichi,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=shahrudir
    Retrieving Results for City #698: Shahrud,IR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=valleyviewca
    Retrieving Results for City #699: Valleyview,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ahlattr
    Retrieving Results for City #700: Ahlat,TR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=farahaf
    Retrieving Results for City #701: Farah,AF
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=barasd
    Retrieving Results for City #702: Bara,SD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=igrimru
    Retrieving Results for City #703: Igrim,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yilancn
    Retrieving Results for City #704: Yilan,CN
    Error Retrieving Results for City #705: Taian, CN. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lesogorskru
    Retrieving Results for City #706: Lesogorsk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kluczborkpl
    Retrieving Results for City #707: Kluczbork,PL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=balezinoru
    Retrieving Results for City #708: Balezino,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=zykovoru
    Retrieving Results for City #709: Zykovo,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dergachiru
    Retrieving Results for City #710: Dergachi,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dumbraveniro
    Retrieving Results for City #711: Dumbraveni,RO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=impfondocg
    Retrieving Results for City #712: Impfondo,CG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=clarksburgus
    Retrieving Results for City #713: Clarksburg,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=uticaus
    Retrieving Results for City #714: Utica,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hamicn
    Retrieving Results for City #715: Hami,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mason cityus
    Retrieving Results for City #716: Mason City,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kuluin
    Retrieving Results for City #717: Kulu,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=marsh harbourbs
    Retrieving Results for City #718: Marsh Harbour,BS
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=vyshestebliyevskayaru
    Retrieving Results for City #719: Vyshestebliyevskaya,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=saint andrewsca
    Retrieving Results for City #720: Saint Andrews,CA
    Error Retrieving Results for City #721: Malakal, SD. Skipping record.
    Error Retrieving Results for City #722: Kapoeta, SD. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sorrentoit
    Retrieving Results for City #723: Sorrento,IT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=half moon bayus
    Retrieving Results for City #724: Half Moon Bay,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ajdabiyaly
    Retrieving Results for City #725: Ajdabiya,LY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dapdapph
    Retrieving Results for City #726: Dapdap,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=san juanus
    Retrieving Results for City #727: San Juan,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pidhorodnaua
    Retrieving Results for City #728: Pidhorodna,UA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sibolgaid
    Retrieving Results for City #729: Sibolga,ID
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=boshnyakovoru
    Retrieving Results for City #730: Boshnyakovo,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=uruzganaf
    Retrieving Results for City #731: Uruzgan,AF
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nautlamx
    Retrieving Results for City #732: Nautla,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nikolskru
    Retrieving Results for City #733: Nikolsk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=aykinoru
    Retrieving Results for City #734: Aykino,RU
    Error Retrieving Results for City #735: Korla, CN. Skipping record.
    Error Retrieving Results for City #736: Kondratovo, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=addankiin
    Retrieving Results for City #737: Addanki,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tiznitma
    Retrieving Results for City #738: Tiznit,MA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=teguldetru
    Retrieving Results for City #739: Teguldet,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lipariit
    Retrieving Results for City #740: Lipari,IT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=road townvg
    Retrieving Results for City #741: Road Town,VG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=wanningcn
    Retrieving Results for City #742: Wanning,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=gallelk
    Retrieving Results for City #743: Galle,LK
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yarimye
    Retrieving Results for City #744: Yarim,YE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=achitru
    Retrieving Results for City #745: Achit,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=balabacph
    Retrieving Results for City #746: Balabac,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=saint-louissn
    Retrieving Results for City #747: Saint-Louis,SN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=biraocf
    Retrieving Results for City #748: Birao,CF
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=san cristobalec
    Retrieving Results for City #749: San Cristobal,EC
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=obeliailt
    Retrieving Results for City #750: Obeliai,LT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kidalml
    Retrieving Results for City #751: Kidal,ML
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=port harcourtng
    Retrieving Results for City #752: Port Harcourt,NG
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=vanderhoofca
    Retrieving Results for City #753: Vanderhoof,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=awbarily
    Retrieving Results for City #754: Awbari,LY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sturgisus
    Retrieving Results for City #755: Sturgis,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=luoyangcn
    Retrieving Results for City #756: Luoyang,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tuy hoavn
    Retrieving Results for City #757: Tuy Hoa,VN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hirarajp
    Retrieving Results for City #758: Hirara,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mandanus
    Retrieving Results for City #759: Mandan,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=middleburyus
    Retrieving Results for City #760: Middlebury,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dakarsn
    Retrieving Results for City #761: Dakar,SN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=flin flonca
    Retrieving Results for City #762: Flin Flon,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=meccasa
    Retrieving Results for City #763: Mecca,SA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=namiejp
    Retrieving Results for City #764: Namie,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=lapypl
    Retrieving Results for City #765: Lapy,PL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ivanovoru
    Retrieving Results for City #766: Ivanovo,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=barcelosbr
    Retrieving Results for City #767: Barcelos,BR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=solnechnyyru
    Retrieving Results for City #768: Solnechnyy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=clearwaterus
    Retrieving Results for City #769: Clearwater,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=vicksburgus
    Retrieving Results for City #770: Vicksburg,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=rorvikno
    Retrieving Results for City #771: Rorvik,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=watrousca
    Retrieving Results for City #772: Watrous,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=araouaneml
    Retrieving Results for City #773: Araouane,ML
    Error Retrieving Results for City #774: Bocaranga, CF. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=villacarrilloes
    Retrieving Results for City #775: Villacarrillo,ES
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=salalahom
    Retrieving Results for City #776: Salalah,OM
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tha maith
    Retrieving Results for City #777: Tha Mai,TH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=viseubr
    Retrieving Results for City #778: Viseu,BR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=nalutly
    Retrieving Results for City #779: Nalut,LY
    Error Retrieving Results for City #780: Toftir, FO. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=fayatd
    Retrieving Results for City #781: Faya,TD
    Error Retrieving Results for City #782: Leonidion, GR. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=jiangyoucn
    Retrieving Results for City #783: Jiangyou,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=purperu
    Retrieving Results for City #784: Purpe,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=boguchanyru
    Retrieving Results for City #785: Boguchany,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=egvekinotru
    Retrieving Results for City #786: Egvekinot,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=valerno
    Retrieving Results for City #787: Valer,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bugasongph
    Retrieving Results for City #788: Bugasong,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=codyus
    Retrieving Results for City #789: Cody,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kaihuacn
    Retrieving Results for City #790: Kaihua,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=north augustaus
    Retrieving Results for City #791: North Augusta,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=distraccionco
    Retrieving Results for City #792: Distraccion,CO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=marzuqly
    Retrieving Results for City #793: Marzuq,LY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=jasperca
    Retrieving Results for City #794: Jasper,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=poronayskru
    Retrieving Results for City #795: Poronaysk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=balakhtaru
    Retrieving Results for City #796: Balakhta,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=north bendus
    Retrieving Results for City #797: North Bend,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=moissalatd
    Retrieving Results for City #798: Moissala,TD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mithipk
    Retrieving Results for City #799: Mithi,PK
    Error Retrieving Results for City #800: Canitas, MX. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=oloron-sainte-mariefr
    Retrieving Results for City #801: Oloron-Sainte-Marie,FR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=borogontsyru
    Retrieving Results for City #802: Borogontsy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=adiakeci
    Retrieving Results for City #803: Adiake,CI
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sofiyivkaua
    Retrieving Results for City #804: Sofiyivka,UA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=grakhovoru
    Retrieving Results for City #805: Grakhovo,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=seahamgb
    Retrieving Results for City #806: Seaham,GB
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=atasukz
    Retrieving Results for City #807: Atasu,KZ
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=cukaimy
    Retrieving Results for City #808: Cukai,MY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=verkhoyanskru
    Retrieving Results for City #809: Verkhoyansk,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dalicn
    Retrieving Results for City #810: Dali,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=surtly
    Retrieving Results for City #811: Surt,LY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ranongth
    Retrieving Results for City #812: Ranong,TH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=gouyavegd
    Retrieving Results for City #813: Gouyave,GD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=topolobampomx
    Retrieving Results for City #814: Topolobampo,MX
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=san josegt
    Retrieving Results for City #815: San Jose,GT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=suhbaatarmn
    Retrieving Results for City #816: Suhbaatar,MN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=atbasarkz
    Retrieving Results for City #817: Atbasar,KZ
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=marfinoru
    Retrieving Results for City #818: Marfino,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=staryy krymua
    Retrieving Results for City #819: Staryy Krym,UA
    Error Retrieving Results for City #820: Arya, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=shachecn
    Retrieving Results for City #821: Shache,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=korhogoci
    Retrieving Results for City #822: Korhogo,CI
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=eurekaus
    Retrieving Results for City #823: Eureka,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sosnovo-ozerskoyeru
    Retrieving Results for City #824: Sosnovo-Ozerskoye,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hezecn
    Retrieving Results for City #825: Heze,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dengzhoucn
    Retrieving Results for City #826: Dengzhou,CN
    Error Retrieving Results for City #827: Kyra, RU. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=tongrencn
    Retrieving Results for City #828: Tongren,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dillonus
    Retrieving Results for City #829: Dillon,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=sovetskiyru
    Retrieving Results for City #830: Sovetskiy,RU
    Error Retrieving Results for City #831: Nam Tha, LA. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=paharpurpk
    Retrieving Results for City #832: Paharpur,PK
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=ingraj bazarin
    Retrieving Results for City #833: Ingraj Bazar,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bonavistaca
    Retrieving Results for City #834: Bonavista,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=totmaru
    Retrieving Results for City #835: Totma,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=unain
    Retrieving Results for City #836: Una,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=whitehorseca
    Retrieving Results for City #837: Whitehorse,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=toyookajp
    Retrieving Results for City #838: Toyooka,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=katsuurajp
    Retrieving Results for City #839: Katsuura,JP
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=cravo norteco
    Retrieving Results for City #840: Cravo Norte,CO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=zavyalovoru
    Retrieving Results for City #841: Zavyalovo,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mattrusl
    Retrieving Results for City #842: Mattru,SL
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=high levelca
    Retrieving Results for City #843: High Level,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=marawisd
    Retrieving Results for City #844: Marawi,SD
    Error Retrieving Results for City #845: Bairiki, KI. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yatoucn
    Retrieving Results for City #846: Yatou,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=chulymru
    Retrieving Results for City #847: Chulym,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=dongningcn
    Retrieving Results for City #848: Dongning,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=shawneeus
    Retrieving Results for City #849: Shawnee,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=indijars
    Retrieving Results for City #850: Indija,RS
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kloulklubedpw
    Retrieving Results for City #851: Kloulklubed,PW
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=shirokiyru
    Retrieving Results for City #852: Shirokiy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=trojeshn
    Retrieving Results for City #853: Trojes,HN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=koraputin
    Retrieving Results for City #854: Koraput,IN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hare bayca
    Retrieving Results for City #855: Hare Bay,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=san vicenteph
    Retrieving Results for City #856: San Vicente,PH
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=jaluly
    Retrieving Results for City #857: Jalu,LY
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bascoph
    Retrieving Results for City #858: Basco,PH
    Error Retrieving Results for City #859: El Balyana, EG. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=athensus
    Retrieving Results for City #860: Athens,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=arkadelphiaus
    Retrieving Results for City #861: Arkadelphia,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=yanggucn
    Retrieving Results for City #862: Yanggu,CN
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=hearstca
    Retrieving Results for City #863: Hearst,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=berkakno
    Retrieving Results for City #864: Berkak,NO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=pangodyru
    Retrieving Results for City #865: Pangody,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=iniridaco
    Retrieving Results for City #866: Inirida,CO
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=bedfordca
    Retrieving Results for City #867: Bedford,CA
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=culebraus
    Retrieving Results for City #868: Culebra,US
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=boendecd
    Retrieving Results for City #869: Boende,CD
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=camachapt
    Retrieving Results for City #870: Camacha,PT
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=batagayru
    Retrieving Results for City #871: Batagay,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mikhaylovskoyeru
    Retrieving Results for City #872: Mikhaylovskoye,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=halmstadse
    Retrieving Results for City #873: Halmstad,SE
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=udachnyyru
    Retrieving Results for City #874: Udachnyy,RU
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=mugur-aksyru
    Retrieving Results for City #875: Mugur-Aksy,RU
    Error Retrieving Results for City #876: Mullaitivu, LK. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=narbonnefr
    Retrieving Results for City #877: Narbonne,FR
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=narasannapetain
    Retrieving Results for City #878: Narasannapeta,IN
    Error Retrieving Results for City #879: Kalavrita, GR. Skipping record.
    Error Retrieving Results for City #880: Geresk, AF. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kayesml
    Retrieving Results for City #881: Kayes,ML
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=kazerunir
    Retrieving Results for City #882: Kazerun,IR
    Error Retrieving Results for City #883: Tawkar, SD. Skipping record.
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=batticaloalk
    Retrieving Results for City #884: Batticaloa,LK
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=agnibilekrouci
    Retrieving Results for City #885: Agnibilekrou,CI
    http://api.openweathermap.org/data/2.5/weather?appid=ecfa6a1cf2efc42a372f2e0aec64ad56&units=Imperial&q=partizanskoyeru
    Retrieving Results for City #886: Partizanskoye,RU
    ------------------------
    Data Retrieval Complete.
    ------------------------





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City Name</th>
      <th>Country Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature (F)</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
      <th>Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>atuona</td>
      <td>pf</td>
      <td>-9.80</td>
      <td>-139.03</td>
      <td>79.78</td>
      <td>100.0</td>
      <td>80.0</td>
      <td>13.91</td>
      <td>1.521043e+09</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bati</td>
      <td>et</td>
      <td>11.19</td>
      <td>40.02</td>
      <td>74.83</td>
      <td>82.0</td>
      <td>68.0</td>
      <td>2.39</td>
      <td>1.521043e+09</td>
    </tr>
    <tr>
      <th>2</th>
      <td>berdigestyakh</td>
      <td>ru</td>
      <td>62.10</td>
      <td>126.70</td>
      <td>-7.70</td>
      <td>61.0</td>
      <td>44.0</td>
      <td>4.63</td>
      <td>1.521043e+09</td>
    </tr>
    <tr>
      <th>3</th>
      <td>butaritari</td>
      <td>ki</td>
      <td>3.07</td>
      <td>172.79</td>
      <td>82.30</td>
      <td>100.0</td>
      <td>56.0</td>
      <td>10.78</td>
      <td>1.521043e+09</td>
    </tr>
    <tr>
      <th>4</th>
      <td>baykit</td>
      <td>ru</td>
      <td>61.68</td>
      <td>96.39</td>
      <td>3.10</td>
      <td>80.0</td>
      <td>56.0</td>
      <td>5.41</td>
      <td>1.521043e+09</td>
    </tr>
  </tbody>
</table>
</div>




```python
unique_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 887 entries, 0 to 886
    Data columns (total 9 columns):
    City Name          887 non-null object
    Country Code       887 non-null object
    Latitude           795 non-null float64
    Longitude          795 non-null float64
    Temperature (F)    795 non-null float64
    Humidity           795 non-null float64
    Cloudiness         795 non-null float64
    Wind Speed         795 non-null float64
    Date               795 non-null float64
    dtypes: float64(7), object(2)
    memory usage: 62.4+ KB



```python
unique_df = unique_df.dropna(how='any')
```


```python
unique_df.count()
```




    City Name          795
    Country Code       795
    Latitude           795
    Longitude          795
    Temperature (F)    795
    Humidity           795
    Cloudiness         795
    Wind Speed         795
    Date               795
    dtype: int64




```python
unique_df.to_csv("unique_cities_lat_longs.csv",
                     encoding="utf-8", index=False)
```


```python
#unique_df[['Latitude','Longitude','Temperature (F)','Humidity','Cloudiness','Wind Speed','Date']] = unique_df[['Latitude','Longitude','Temperature (F)','Humidity','Cloudiness','Wind Speed','Date']].apply(pd.to_numeric)                                    
```


```python
plt.scatter(x='Latitude', y='Temperature (F)', c="cyan", alpha=0.75, edgecolor="black", s=25, data=unique_df)  

plt.title("Latitude vs. Max. Temperature (F) (03/14/2018)")
plt.ylabel("Max. Temperature (F)")
plt.xlabel("Latitude")
    
plt.ylim(-100, 150)
plt.xlim(-60, 100)

plt.savefig("Latitude_MaxTemp.png", dpi=150)

plt.show()
```


![png](output_19_0.png)



```python
plt.scatter(x='Latitude', y='Humidity', c="cyan", alpha=0.75, edgecolor="black", s=25, data=unique_df)  
          
plt.title("City Latitude vs. Humidity (03/14/2018)")
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
    
plt.ylim(-20, 120)
plt.xlim(-60, 100)

plt.savefig("Latitude_Humidity.png", dpi=150)

plt.show()
```


![png](output_20_0.png)



```python
plt.scatter(x='Latitude', y='Cloudiness', c="cyan", alpha=0.75, edgecolor="black", s=25, data=unique_df)  
          
plt.title("City Latitude vs. Cloudiness (03/14/2018)")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
    
plt.ylim(-20, 120)
plt.xlim(-60, 100)

plt.savefig("Latitude_Cloudiness.png", dpi=150)

plt.show()
```


![png](output_21_0.png)



```python
plt.scatter(x='Latitude', y='Wind Speed', c="cyan", alpha=0.75, edgecolor="black", s=25, data=unique_df)  
          
plt.title("City Latitude vs. Wind Speed (03/14/2018)")
plt.ylabel("Wind Speed (mph)")
plt.xlabel("Latitude")
    
plt.ylim(-10, 50)
plt.xlim(-60, 100)

plt.savefig("Wind_Speed.png", dpi=150)

plt.show()
```


![png](output_22_0.png)

