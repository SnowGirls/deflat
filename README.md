# deflat


Cause [angr](https://github.com/angr/angr) & [BARF](https://github.com/programa-stic/barf-project) have refactor some API . Update the python scripts in the following artical :

[(腾讯安全应急响应中心 Tencent Security Response Center) 博客 利用符号执行去除控制流平坦化](https://security.tencent.com/index.php/blog/msg/112)
 
## requirements

* Install [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/)
* Install [angr](https://github.com/angr/angr) in virtualenv.
* Install [BARF](https://github.com/programa-stic/barf-project) in virtualenv.

## usage

Open your terminal and issue `mkvirtualenv angrenv` or `workon angrenv`
and `cd` to your scripts directory, then issue `python deflat.py check_passwd_flat 0x400530`

Note the address (of function *check_password*) `0x400530` is copied from IDA/Hopper. Following is the output:

```javascript
(angrenv) MacBookPro$ python deflat.py check_passwd_flat 0x400530

*******************relevant blocks************************
prologue:0x400530
main_dispatcher:0x400554
pre_dispatcher:0x40099b
retn:0x40098f
relevant_blocks: ['0x4007ec', '0x40080d', '0x400819', '0x400837', '0x40084e', '0x40086a', '0x400886', '0x4008a9', '0x4008cc', '0x4008ee', '0x40091b', '0x40092e', '0x40094f', '0x40095b', '0x40097c']
*******************symbolic execution*********************
-------------------dse 0x4007ec---------------------
-------------------dse 0x40080d---------------------
-------------------dse 0x400819---------------------
-------------------dse 0x400837---------------------
-------------------dse 0x40084e---------------------
-------------------dse 0x40086a---------------------
-------------------dse 0x400886---------------------
-------------------dse 0x4008a9---------------------
-------------------dse 0x4008cc---------------------
-------------------dse 0x4008ee---------------------
-------------------dse 0x40091b---------------------
-------------------dse 0x40092e---------------------
-------------------dse 0x40094f---------------------
-------------------dse 0x40095b---------------------
-------------------dse 0x40097c---------------------
-------------------dse 0x400530---------------------
************************flow******************************
0x40095b: ['0x40097cL']
0x400886: ['0x4008a9L', '0x40094fL']
0x40098f: []
0x4008a9: ['0x4008ccL', '0x40094fL']
0x40086a: ['0x400886L', '0x40094fL']
0x4007ec: ['0x400819L', '0x40080dL']
0x40080d: ['0x40084eL']
0x40084e: ['0x40086aL', '0x40095bL']
0x40094f: ['0x40097cL']
0x400530: ['0x4007ecL']
0x40092e: ['0x40094fL']
0x4008cc: ['0x4008eeL', '0x40094fL']
0x4008ee: ['0x40091bL', '0x40092eL']
0x400837: ['0x4007ecL']
0x400819: ['0x400837L']
0x40091b: ['0x40098fL']
0x40097c: ['0x40098fL']
************************patch*****************************
Successful! The recovered file: check_passwd_flat.recovered

(angrenv) MacBookPro$
```

