##webalchemy
webalchemy is a light and Pythonic framework to build interactive webapps. It is open source (MIT licensed) you can use it in commercial software. The library requires Python >= 3.3, and Tornado >= 3.1. Note it is currently in early stages of development, still far from being feature complete. webalchemy webapps require a modern browser supporting some websockets.

webalchemy represents a novel approach to webapp development obviously inspired by [sqlalchemy](http://www.sqlalchemy.org/), and the IPython [notebook](http://ipython.org/). It let's you write server code as if it were running on the client Err.., In fact forget about client and server altogether. Develop like you would for the desktop.

##Develop a webapp like you would a desktop app

webapps made with webalchemy are highly dynamic: the browser and server frequently exchange messages over websockets. Only a minimal amount of static content is initially served, and from there on all page is rendered dynamically. This allows to keep fronend code simple as all complex logic is implemented on the server, using regular Python. In essense, with webalchemy you use Python to automate the frontend. Simple library calls generate the JS required on the frontend, and you program like you would a desktop app - not caring much about HTML, CSS, or JS.

####Example
```python
from tornado import gen
class my_app:    
    def __init__(self, rdoc):
        self.p= rdoc.create_element('p',txt='This is an empty document', )
        rdoc.root_append(self.p)
        self.i= rdoc.create_interval(1000,rdoc.msg('interval!'))
        self.i.count=0
        rdoc.begin()
        e=rdoc.create_element('p',txt=':)', )
        rdoc.root_append(e)
        rdoc.end()
        self.i2= rdoc.create_interval(2500)
    
    @gen.coroutine
    def message(self, rdoc, txt):
        if txt!='interval!':
            return
        if self.i.count>5:
            self.i.stop()
            self.i2.stop()
        self.i.count+=1
        p= rdoc.create_element('p',txt='New paragraph #'+str(self.i.count))
        p.set_style_att('position','absolute')
        p.set_style_att('left',str(50*self.i.count)+'px')
        p.set_style_att('top',str(50*self.i.count)+'px')
        self.p.set_text('there are now '+str(self.i.count+1)+' paragraphs')
        rdoc.root_append(p)
            
if __name__=='__main__':
    import webalchemy.server
    server.run(8083,my_app)
```
##Philosophy
Lightweight - does one thing and does it well!

Pure Python - as much a possible for webapps

Non oppinionated - plays well with others

High performance









