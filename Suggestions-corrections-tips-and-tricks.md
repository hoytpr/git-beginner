---

---
[Readme (home)](./README.md)

### These are some discussion items that could be useful. 

- **Use of "GitHub Online"**: Just wondering what is meant by this term? Perhaps 'user fork' or 'your fork' of a SWC repo might be clearer?
	- This was referring to the users own repos that are **on Github**. They are not the "local" repos on their computers, nor are they the Carpentries, (or other) repos.
	- Saying "your fork" of a Carpentries repo would be perfect (just more typing. I'm not a typist).
- **Meaning of 'upstream' and 'origin' is reversed**: These notes indicate that the user's fork of the Carpentries repo is called 'upstream'. This is the opposite to the normal naming convention, where the Carpentries would be referred to as the upstream repo (changes flow downstream to the user fork ('origin')).
	- This is a great point! The protocols are set up where the user first clones the repo from the Carpentries site to their local computer, which has always (in my limited experience) automatically made the Carpentries repo the "origin". 
	- So if you first fork the repo, then clone your OWN repo down to your local computer, the Carpentries repo would then be set as the "upstream". This really needs to be addressed for newbies or it could be a problem! Thanks.
- **Using `gh-pages`:**  It is possible gh-pages may not be used in some new manifestations of the lessons. The "master" branch will be used instead.
	- This is just something we'll have to deal with. The gh-pages will still be used for rendering... maybe? It will need to posted in the README of the repos IMHO. 
	- One solution is to substitute "master" wherever you see "gh-pages" in these protocols.
	- Better solutions are welcome!
- **Why write about fixing your Github repo `gh-pages`** when commits get ***ahead*** of the Carpentries repo `gh-pages`?
	- Because this took me nearly *two years* to figure out. Seriously. The disconnect was that as a COMPLETE beginner, I asked how to "fix" things that had gone wrong. Experienced users would answer by telling me the correct way to use GIT. These are *two different things*. As a life scientist I couldn't ask the question correctly, and the answer was too obvious.

