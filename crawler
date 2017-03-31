#The idea behind this code is relatively simple:
    #Branching out from one Wikipedia article (for example, Compton Scattering), list
    #all article hrefs. Query if they are physics related, then continue iterating over the list.
    #When the page is complete, move to the next page and do the same thing.
    #For reasonability with speed, check if term has already been deemed physics related.

#My personal "physics article" standards:
# Article must be either a theoretical concept, experimental method, or mathematical approach.
# Basically, technical physics.
# This is fairly strict. For example, these are not "physics articles":
    # Arthur Holly Compton (inventor of Compton Scattering - related to history of science.)
    # Nobel Prize in Physics (while obviously important to physics, not related directly to practice of science)
    # Physical Review (while journals are important, they are not for the practice of pure physics)
# There is some gray area, but try to keep it confined to the concepts above.

from selenium import webdriver
import time

#Removing all current progress (Done during debugging/writing process - left here for problem analysis if necessary)
# f = open('wikipedia_physics.txt','w')
# g = open('wikipedia_physics_rejected.txt','w')
# h = open('wikipedia_finished.txt','w')
# f.close()
# g.close()
# h.close()

#Don't forget to replace this with your ChromeDriver directory
browser = webdriver.Chrome(r'[your ChromeDriver directory]')
browser.implicitly_wait(4)

#Check which article to look at right now
with open('wikipedia_physics.txt') as t:
    possible = t.readlines()
    possible = [x.strip('\n') for x in possible]
with open('wikipedia_finished.txt') as t:
    not_possible = t.readlines()
    not_possible = [x.strip('\n') for x in not_possible]

for k in possible:
    #Conditional which decides if it's been seen yet
    if k not in not_possible:
        current = k
        break

#This is just in case the text file is blank
try:
    current
except:
    current = 'https://en.wikipedia.org/wiki/Compton_scattering'

print('Current article:' + '\n' + current + '\n')

browser.get(current)
browser.implicitly_wait(4)

elems = browser.find_elements_by_xpath('//*[@id="mw-content-text"]//a[@href]')
#First part of xpath is to select only the article area, second is for links
urls = [str(x.get_attribute('href')) for x in elems] #Clean to only URLS
link_text = [str(x.get_attribute('text')) for x in elems] #Link text

#Cleaning non-article links
good_urls, good_link_text = [], []
for i,x in enumerate(urls):
    # If you're wondering why I didn't just use ':' as my exception case, see this legitimate page:
    # https://en.wikipedia.org/wiki/Final_Fantasy:_The_Spirits_Within
    #Probably better to use a function instead (for clarity) but this works.
    if x[:30] == 'https://en.wikipedia.org/wiki/' and x[:len(current)] != current and x[30:35] != 'File:' and x[30:39] != 'Template:' and x[30:44] != 'Template_talk:' and x[30:38] != 'Special:' and x[30:35] != 'Help:' and x[30:40] != 'Wikipedia:' and x[30:37] != 'Portal:':
        good_urls.append(x)
        good_link_text.append(link_text[i])

#Previous list comprehension rules left here to aid future debugging:
#(Please update these if you change the long if statement.)
#     fix1 = [x for x in urls if x[:30] == 'https://en.wikipedia.org/wiki/'] #No outside links
#     fix2 = [x for x in fix1 if x[:len(current)] != current] #No self links
#     fix3 = [x for x in fix2 if x[30:35] != 'File:'] #No file links
#     fix4 = [x for x in fix3 if x[30:39] != 'Template:'] #No template links
#     fix5 = [x for x in fix4 if x[30:44] != 'Template_talk:'] #Alternate template link
#     fix6 = [x for x in fix5 if x[30:38] != 'Special:'] #No special links
#     fix7 = [x for x in fix6 if x[30:35] != 'Help:'] #No help links
#     fix8 = [x for x in fix7 if x[30:40] != 'Wikipedia:'] #No Wikipedia internal links
#     fix9 = [x for x in fix8 if x[30:37] != 'Portal:'] #No portal links

#Query if physics or not
def query(i): #In a function for clarity later
    # *NOT* functionally programmed
    f = open('wikipedia_physics.txt','a')
    c = open('wikipedia_physics_rejected.txt','a')
    #Somewhat sloppy text file management, I know, but it works.
    
    print(good_link_text[i])
    print(good_urls[i])
    ans = input('Is this a physics article? (y/n) ')
    if ans == 'y':
        f.write(good_urls[i] + '\n')
    if ans == 'quit': #Safe quit that saves current answers
        f.write('\n')
        f.close()
        return('break')
    if ans == 'n':
        c.write(good_urls[i] + '\n')
    f.close()
    print('\n')
    return 'no issues'

#The main workhorse - asks the query function
check_rejects = 1
for i in range(len(good_urls)):
    with open('wikipedia_physics.txt') as g:
        previous = g.readlines()
        previous = [x.strip('\n') for x in previous]
    if check_rejects == 1:
        with open('wikipedia_physics_rejected.txt') as h:
            rejects = h.readlines()
            rejects = [x.strip('\n') for x in rejects]
        #checkpound(good_urls[i]) #Checking for pound sign could reduce redundant errors, but what about
                                  #edge cases?
        if good_urls[i] not in previous + rejects:
            status = query(i) #Asks "is this physics?"
            if status == 'break':
                break
    else:
        if good_urls[i] not in previous:
            status = query(i) #Asks "is this physics?"
            if status == 'break':
                break

#Only writes if it has searched through all the article urls on the page.
#So you have to manually rerun the code when you switch to a new article
if status == 'no issues':
    d = open('wikipedia_finished.txt','a')
    d.write(current + '\n')
    d.close()
