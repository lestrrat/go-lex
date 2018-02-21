go-lex
======

[![Build Status](https://travis-ci.org/lestrrat/go-lex.svg?branch=master)](https://travis-ci.org/lestrrat/go-lex)

This is a simple lexer, based loosely on `text/template`'s lexer.

# WARNING

This repository has been moved to [github.com/lestrrat-go/lex](https://github.com/lestrrat-go/lex). This repository exists so that libraries pointing to this URL will keep functioning, but this repository will NOT be updated in the future. Please use the new import path.

## HOW TO USE

The lexing is done by chaining `lex.LexFn` functions. Create a `StringLexer` or a `ReaderLexer`, and pass it an entry point to start lexing. The result will be passed through a channel as a series of `lex.Item`s:

```go
l := NewStringLexer(buf, lexStart)
go l.Run()

for item := range l.Items() {
   // Do whatever
}
```

In your lexing functions, you should do whatever processing necessary, and return the next lexing function. If you are done and want the lexing to stop, return a `nil` for `lex.LexFn`

```go
func lexStart(l lex.Lexer) lex.LexFn {
  if !l.AcceptString("Hello") {
    l.EmitErrorf("expected 'Hello'")
    return nil
  }
  
  if !l.AcceptRun(" ") {
    l.EmitErrorf("expected space")
    return nil
  }
    
  return lexWorld
}

func lexWorld(l lex.Lexer) lex.LexFn {
  if !l.AcceptString("World") {
    l.EmitErrorf("expected 'World'")
    return nil
  }
  // In reality we should check for EOF, but for now, we just end processing
  return nil
}
```


  
