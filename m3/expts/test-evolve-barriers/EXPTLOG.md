# Purpose

To plot results of an evolutionary run using the `evolve-bridge-and-barrier-tests-X.cfg` config
file, which does an initial evolutionary test of evolving barrier positions.
It uses flower-min/max-visit-count-success as the objective function rather than EMD distance.

The evolutionary run uses 3 islands each running the sga algorithm, with migration period 20.

# Code

Run the experiment with the following command:

```
./run-release -c config-files/evolve-bridge-and-barrier-tests-X.cfg | tee out-evolve-bridge-and-barrier-tests-X.txt
```

From the output of the above, make a CSV file containing island number, generation number and fitness score for each evaluation:

```
gawk '/INFO: isl/ {print $3","$5","$11}' out-evolve-bridge-and-barrier-tests-X.txt > out-evolve-bridge-and-barrier-tests-X.csv
```

Generate lists of fitness achieved in each run:

```
grep "Champion fitness: -" out*.txt > fitness-list-raw.txt
grep "Champion fitness: -" out*.txt|gawk '{print $3}'|sort > fitness-list-sorted.txt
```

Now plot this data:

```
~/polybee/tools/plot_fitness_islands.py --type 1 --title "Evolve Barrier Positions" out-evolve-bridge-and-barrier-tests-X.csv
```

and then save the plot via the GUI of the `plot_fitness_islands.py` script, and rename it:

```
mv ~/Figure_1.png out-evolve-bridge-and-barrier-tests-X.png
```

and to create a config file of the winning design:

```
~/polybee/tools/best_individual_to_cfg.py out-evolve-bridge-and-barrier-tests-X.txt
```

To generate heatmaps of positioning of barriers and bridges:

```
~/polybee/tools/gen_bx_heatmaps.py --cell-size 25 --basename heatmap-200-gen-20-X-all 200-gen-evolve-BX-*-20-X*/best*cfg

~/polybee/tools/visualize_heatmap.py --title "Heatmap of barrier positions over all 200 generation runs" --config 200-gen-evolve-BX-200-pop-2000-its-20-X/200-gen-evolve-BX-200-pop-2000-its-20-X.cfg heatmap-200-gen-20-X-all_barriers.csv

```
