# Playlist Chaos

Your AI assistant tried to build a smart playlist generator. The app runs, but some of the behavior is unpredictable. Your task is to explore the app, investigate the code, and use an AI assistant to debug and improve it.

This activity is your first chance to practice AI-assisted debugging on a codebase that is slightly messy, slightly mysterious, and intentionally imperfect.

You do not need to understand everything at once. Approach the app as a curious investigator, work with an AI assistant to explain what you find, and make targeted improvements.

---

## Summary of Tinker Task

The purpose of this Tinker activity is to help students learn how to utilize AI tools like Claude or GitHub Copilot as well as how to use GitHub command lines. It asks students to first manually test how the product works and think about anything that feels off. The students are asked to use AI tools to help them understand any code that's incomplete or leads to confusion, but it emphasizes that AI answers should be used as hypotheses only and not a final answer. This is important as students utilize the AI tools to help them debug. For students who are not familiar with Python, asking AI will help them locate code that looks weird. Then, students are asked to pick one problem/confusion that they see from experiencing with the website, which they should start by picking a simple issue to fix. It asks the students to look through app.py or playlist_logic.py for the code section that's responsible for the issue. Once a student finds the issue, students are asked to highlight the section and ask AI to fix it. This part might be difficult for some students who are not familiar with Python, as looking through the code is difficult without knowing the internal functions that Python includes. Therefore, students should use AI to help them identify the correct section of the code and ask AI to explain what each line means, as it will help them understand better. 

Personally, the exercise was confusing at the start since the website contains a lot of information and it overwhelmed me. However, the "How the code is organized" section really helped me as it hints at how I can test the website and mentions some areas that might have issues. AI was also very helpful as I highlighted certain sections and asked AI if the function makes sense. If not, I would ask AI to explain line by line and tell me what the issue would be. The best way is to look at the description of the task and experience the website, then find something that doesn't make sense and look for it in the codebase. From there, you can ask AI to help you identify problems and locate where the issue might be.

---

## Issues Found

### Issue 1: `hype_ratio` always returns 1.0

**Original code (`playlist_logic.py`, line 119):**
```python
total = len(hype)
hype_ratio = len(hype) / total if total > 0 else 0.0
```

**Problem:** `total` is set to `len(hype)`, so `len(hype) / total` is dividing a number by itself — always producing `1.0` whenever the Hype playlist is non-empty. This makes the stat meaningless.

**Fix:** Use `len(all_songs)` as the denominator so the ratio reflects what fraction of all songs are in the Hype playlist:
```python
total = len(all_songs)
hype_ratio = len(hype) / total if total > 0 else 0.0
```

---

### Issue 2: `avg_energy` uses the wrong list

**Original code (`playlist_logic.py`, line 124):**
```python
total_energy = sum(song.get("energy", 0) for song in hype)
avg_energy = total_energy / len(all_songs)
```

**Problem:** The numerator sums energy only from the `hype` playlist, but the denominator divides by `len(all_songs)`. This mixes two different scopes and produces an incorrect average — neither the average energy of all songs nor the average energy of just the Hype playlist.

**Fix:** Sum energy across `all_songs` to match the denominator:
```python
total_energy = sum(song.get("energy", 0) for song in all_songs)
avg_energy = total_energy / len(all_songs)
```

---

### Issue 3: `search_songs` condition is backwards

**Original code (`playlist_logic.py`, line 171):**
```python
if value and value in q:
```

**Problem:** This checks if the song's field value is a substring of the query, not the other way around. For example, searching `"tay"` would test `"taylor swift" in "tay"`, which is `False` — so no results are returned. Partial search never works.

**Fix:** Flip the `in` check so the query is matched inside the field value:
```python
if value and q in value:
```

---

### Issue 4: `lucky_pick` never includes the Mixed playlist

**Original code (`playlist_logic.py`, line 186):**
```python
else:
    songs = playlists.get("Hype", []) + playlists.get("Chill", [])
```

**Problem:** The `"any"` mode (the default) only combines Hype and Chill songs. Songs classified as `"Mixed"` are completely excluded from lucky pick, regardless of the mode chosen.

**Fix:** Include the Mixed playlist in the `else` branch:
```python
else:
    songs = playlists.get("Hype", []) + playlists.get("Chill", []) + playlists.get("Mixed", [])
```

---

## How the code is organized

### `app.py`  

The Streamlit user interface. It handles things like:

- Showing and updating the mood profile  
- Adding songs  
- Displaying playlists  
- Lucky pick  
- Stats and history

### `playlist_logic.py`  

The logic behind the app, including:

- Normalizing and classifying songs  
- Building playlists  
- Merging playlist data  
- Searching  
- Computing statistics  
- Lucky pick mechanics

You will need to look at both files to understand how the app behaves.

---

## What you will do

### 1. Explore the app  

Run the app and try things out:

- Add several songs with different titles, artists, genres, and energy levels  
- Change the mood profile  
- Use the search box  
- Try the lucky pick  
- Inspect the playlist tabs and stats  
- Look at the history  

As you explore, write down at least five things that feel confusing, inconsistent, or strange. These might be bugs, quirks, or unexpected design decisions.

### 2. Ask AI for help understanding the code  

Pick one issue from your list. Use an AI coding assistant to:

- Explain the relevant code sections  
- Walk through what the code is supposed to do  
- Suggest reasons the behavior might not match expectations  

For example:

> "Here is the function that classifies songs. The app is mislabeling some songs. Help me understand what the function is doing and where the logic might need adjustment."

Before making changes, summarize in your own words what you think is happening.

### 3. Fix at least four issues  

Make improvements based on your investigation.

For each fix:

- Identify the source of the issue  
- Decide whether to accept or adjust the AI assistant's suggestions  
- Update the code  
- Add a short comment describing the fix  

Your fixes may involve logic, calculations, search behavior, playlist grouping, lucky pick behavior, or anything else you discover.

### 4. Test your changes  

After each fix, try interacting with the app again:

- Add new songs  
- Change the profile  
- Try search and stats  
- Check whether playlists behave more consistently  

Confirm that the behavior matches your expectations.

### 5. Optional stretch goals  

If you finish early or want an extra challenge, try one of these:

- Improve search behavior  
- Add a "Recently added" view  
- Add sorting controls  
- Improve how Mixed songs are handled  
- Add new features to the history view  
- Introduce better error handling for empty playlists  
- Add a new playlist category of your own design  

---

## Tips for success

- You do not need to solve everything. Focus on exploring and learning.  
- When confused, ask an AI assistant to explain the code or summarize behavior.  
- Test the app often. Small experiments reveal useful clues.  
- Treat surprising behavior as something worth investigating.  
- Stay curious. The unpredictability is intentional and part of the experience.

When you finish, Playlist Chaos will feel more predictable, and you will have taken your first steps into AI-assisted debugging.
