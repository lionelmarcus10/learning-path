# John The Ripper

```bash
# basic format 
john --format=<hash_type> <hash or hash_file>

# show all formats 
john --list=formats


# additional wordlist :  
--wordlist <WordlistFile>

# add rules 
--rules <RuleFile>

# incremental mode : try all possibilities 
john --incremental <hash_file>

# find all convertor to john
locate *2john*
# transform file to john format in order to crack it (..2john)
<tool> <file_to_crack> > <OutputJohnFile>.hash

    # crack it 
    john <OutputJohnFile>.hash

# more
john --show=<hidden-options | types | formats>
```

##