 # Универсальность консенсуса, универсальная lock-free и wait-free конструкция, управление памятью



### Универсальная lock-free конструкция

```
my = ... // create node with method invocation

while (my->seqnum == 0) {
 before = ReadTail()
 after = before->consensus.Decide(my)
 before->next = after
 after->seqnum = before->seqnum + 1
 tail[t] = after
}

```

### Универсальная wait-free конструкция

```
my = ... // create node with method invocation
announce[t] = my
tail[t] = ReadTail()

while (my->seqnum == 0) {
 before = tail[t]
 turn = (before->seqnum + 1) % num_threads
 
 if (announce[turn]->seqnum == 0)
  candidate = announce[turn]
 else
  candidate = my
  
 after = before->consensus.Decide(candidate)
 before->next = after
 after->seqnum = before->seqnum + 1
 tail[t] = after
}

tail[t] = my
```
