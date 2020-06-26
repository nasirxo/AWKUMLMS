# AWKUMLMS
AWKUM LMS AUTOMATED SCRIPT


```py

'''


   AUTOMATED AWKUM LMS ATTENDENCE SCRIPT
   ---------------------------------------
       BY : NASIR ALI
       EMAIL : nasiralis1731@gmail.com
       Facebook : fb.com/nasir.xo
       Github : github.com/nasirxo
   ---------------------------------------


'''
import os
import json
try:
 from requests import *
 from bs4 import BeautifulSoup as bs
except:
  os.system('pip install requests')
  os.system('pip install bs4')

url = 'https://awkumlms.com/{}'
curl = url.format('Student/Enter-classroom?')


if not os.path.exists('.c'):
  e = input(' Email : ')
  p = input(' Password : ')
  CH = json.dumps({'email':e,'pass':p})
  with open('.c','w') as Z:
    Z.write(CH)

LD = json.loads(open('.c','r').read())

data = {
        'email':LD['email'],
        'pass':LD['pass'],
        'check':'hkjhg123098',
        'submit':'Sign In'
        }

s = Session()

r = s.post(url.format('Login/validate_login'),data)

def login(e,p):
  C = {
        'email':e,
        'pass':p,
        'check':'hkjhg123098',
        'submit':'Sign In'
        }
  S = Session()
  G = S.post(url.format('Login/validate_login'),C)
  return G


phy = {
    'class_id':'584',
    'classname':'BS-2-Electricity & Magnetism',
    'faculty_id':'313',
    'submit':'Enter'
}

prog = {
   'class_id':'592',
   'classname':'BS-2-Introduction to Programing',
   'faculty_id':'7',
   'submit':'Enter'
}

math = {
    'class_id':'1520',
    'classname':'BS-2-Calculus II',
    'faculty_id':'613',
    'submit':'Enter'
}

eng = {
   'class_id':'1252',
   'classname':'BS-2-Communications Skills',
   'faculty_id':'557',
   'submit':'Enter'
}


soc = {
    'class_id':'1456',
    'classname':'BS-2-Principle of sociology ',
    'faculty_id':'559',
    'submit':'Enter'
}


def FIL(x):
 C = []
 for J in x:
   if J not in C:
      C.append(J)
 return C



def getF(X):
  CD = {}
  for Y in X('input'):
    try: CD[Y.get('name')]=Y.get('value')
    except: pass
  return CD


def getdata():
  Q = s.get(url.format('Student/Classroom'))
  f = bs(Q.content,'html.parser').findAll('form')[1:]
  return map(getF,FIL(f))



def join(c):
 D = {
    'class_id':c.get('class_id')
 }
 r = s.post(curl,c)
 k = s.post(url.format('Student/function?type=joinclass'),D)
 return k


#A_SUB = [phy,prog,math,eng,soc]
A_SUB = list(getdata())


def joinall():
  for Z in A_SUB:
   U = join(Z)
   if 'zoom' in U.url:
     print('''
--------------------------------------------
 SUBJECT : {}
 URL : {}
--------------------------------------------
     '''.format(Z.get('classname'),U.url))


def R(n):
 n = n.replace('\n','')
 return n.replace('  ','')


def T2T(table):
  results = {}
  T = table.findAll('td')
  for Q in range(len(T)):
   if Q%2==0:
     try:
      results[R(T[Q].get_text())] = R(T[Q+1].get_text())
     except: pass
  PD = ""
  for c in results.keys():
    try:
     PD+=c.upper()+' '+results.get(c).upper()+'\n'
    except: pass
  return PD


def info():
  table = bs(r.content,'html.parser').find('table')
  print(T2T(table))

def Aupload(c,a_id,file):
  r = s.post(curl,c)
  D = {
    'class_id':c.get('class_id'),
    'input_assignment_id':str(a_id),
    'attachment':file,
    'submit':upload
  }



print(" Total Subject's : {}".format(len(A_SUB)))
print('--'*20)
print('---------[ Student Information ]------------')
info()
print('--'*20)
print('---------[ Online Classes ]------------')

```
