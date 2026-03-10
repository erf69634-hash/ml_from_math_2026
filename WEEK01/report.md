# kaggle_python_notes.md

---

## 1. Libraries - word_search

def word_search(doc_list, keyword):
    result = []
    kw = keyword.lower()
    for i, val in enumerate(doc_list):
        words = [w.strip(',.').lower() for w in val.split()]
        if kw in words:
            result.append(i)
    return result

---

## 2. External Libraries - prettify_graph

def prettify_graph(graph):
    graph.set_title("Results of 500 slot machine pulls")
    graph.set_ylim(0)
    graph.set_ylabel("Balance")
    ticks = graph.get_yticks()
    graph.set_yticks(ticks)
    new_ylabels = ['${}'.format(int(amt)) for amt in ticks]
    graph.set_yticklabels(new_ylabels)

graph = jimmy_slots.get_graph()
prettify_graph(graph)

## best_items - Bug Fix

from learntools.python.luigi_analysis import full_dataset

def best_items(racers):
    winner_item_counts = {}
    for i in range(len(racers)):
        racer = racers[i]
        if racer['finish'] == 1:
            for item in racer['items']:
                if item not in winner_item_counts:
                    winner_item_counts[item] = 0
                winner_item_counts[item] += 1
        if racer['name'] is None:
            print("WARNING: racer {}/{}".format(i+1, len(racers)))
    return winner_item_counts

best_items(full_dataset)

---

## 3. Strings - Escape Characters

| Input | Output | Example | print() result |
|-------|--------|---------|----------------|
| \' | ' | 'What\'s up?' | What's up? |
| \" | " | "That's \"cool\"" | That's "cool" |
| \\ | \ | "mountain: /\\" | mountain: /\ |
| \n | newline | "1\n2 3" | 1 / 2 3 |

claim = "Pluto is a planet!"
claim.startswith("Pluto")  # True
claim.endswith("planet!")  # True
'/'.join([month, day, year])  # '01/31/1956'

"{}, you'll always be the {}th planet to me.".format(planet, position)
"{} weighs about {:.2} kg ({:.3%} of Earth). {:,} Plutonians.".format(
    planet, pluto_mass, pluto_mass/earth_mass, population)

---

## 4. Python List Comprehension

playout = [play_slot_machine()-1 for i in range(n_runs)]
return sum(playout) / n_runs

# Equivalent for loop:
playout = []
for i in range(n_runs):
    playout.append(play_slot_machine()-1)

# estimate_average_slot_payout(1) -> -1 or 0.5
# estimate_average_slot_payout(10000000) -> 0.02430565

---

## 5. List Comprehensions - with if filter

short_planets = [planet for planet in planets if len(planet) < 6]
# ['Venus', 'Earth', 'Mars']

loud_short_planets = [planet.upper() + '!' for planet in planets if len(planet) < 6]
# ['VENUS!', 'EARTH!', 'MARS!']

def elementwise_greater_than(L, thresh):
    return [x > thresh for x in L]
# elementwise_greater_than([1,2,3,4], 2) -> [False, False, True, True]
