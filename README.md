<div align="center">

## A Simple Grammar


</div>

### Description

An example of a primitive translator, based on a true grammar. Converts an ariphmetic expression to a backward polish notation.
 
### More Info
 
an ariphmetic ( only numbers and signs, no spaces) expression

this prg. uses watcom-specific string classes. use Watcom C++ compiler or change String type to CString. may require some minor changes in code


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Roman "Krishna" Bystritskiy](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/roman-krishna-bystritskiy.md)
**Level**          |Beginner
**User Rating**    |5.0 (10 globes from 2 users)
**Compatibility**  |C\+\+ \(general\)
**Category**       |[Algorithms](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/algorithms__3-29.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/roman-krishna-bystritskiy-a-simple-grammar__3-2309/archive/master.zip)

### API Declarations

```
/* (c) Roman "Krishna" Bystritsky, 2001 */
/* file agram.h */
#ifndef AGRAM_H
#define AGRAM_H
#include "string.hpp"
int	parseExpression( String& in, String& out);
// for agram.cpp use only
int	parseFormula( String& in, String& out, int& infix);
int	parseTerm	( String& in, String& out, int& infix);
int	parseElem	( String& in, String& out, int& infix);
#define IsPM(c) ((c)=='+' || (c)=='-')
#define IsMD(c) ((c)=='*' || (c)=='/')
#define IsDigit(c) ((c)>='0' && (c) <= '9')
#define IsOB(c) ((c)=='(')
#define IsCB(c) ((c)==')')
#define RET_ERR 1
#define RET_OK	0
#define BLANK(c) ((c)==' ' || (c) == '\n' || (c)=='\r' || (c)=='\t')
#endif
```


### Source Code

```
/* (c) Roman "Krishna" Bystritsky, 2001 */
/* file agram.cpp */
#include "string.hpp"
#include "iostream.h"
#include "stdio.h"
#include "agram.h"
/*
 * Grammar:
 *
 *	<formula> -> <term> { +/- <term> }
 * <term>	 -> <element> { *\'/' <element> }
 *	<element> -> ( <formla> ) | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 0
 *	+/-	  ->	+		|	-
 * * \ '/' -> *		| /
**/
/*	<formula> -> <term> { +/- <term> } */
int	parseFormula( String& in, String& out, int& infix)
{
	char	cSign;
	if(parseTerm(in,out, infix))
 {
 	cout << "From parseFormula"<<endl;
  return RET_ERR;
 }
 while( IsPM(cSign=in[infix]) )
 {
 	infix++;
  if(parseTerm(in,out,infix))
  {
	 	cout << "From parseFormula"<<endl;
 	 return RET_ERR;
  }
  out += cSign;
  out += ' ';
 }
 return RET_OK;
}
/* <term>	 -> <element> { *\'/' <element> } */
int	parseTerm(String& in, String& out, int& infix)
{
	char	cSign;
	if(parseElem(in,out,infix))
 {
 	cout << "From parseTerm" << endl;
  return RET_ERR;
 }
 while( IsMD(cSign=in[infix]) )
 {
 	infix++;
  if(parseElem(in,out,infix))
  {
	 	cout << "From parseTerm"<<endl;
 	 return RET_ERR;
  }
  out += cSign;
  out += ' ';
 }
	return RET_OK;
}
/*	<element> -> ( <formla> ) | digit+*/
int	parseElem(String& in, String& out, int& infix)
{
	char cSign;
	cSign = in[infix];
	if(IsOB(cSign))	// case of ( formula )
 {
  infix ++;
 	if(parseFormula(in,out, infix))
  {
  	cout << "from parseElem" << endl;
   return RET_ERR;
  }
  cSign = in[infix];
  infix++;
  if(!IsCB(cSign))
  {
  	cout << "parseElem: missing close brace c. " << infix+1 <<" "<<endl;
   return RET_ERR;
  }
 } else
 {
 	while(IsDigit(cSign))
  {
  	out += cSign;
   cSign= in[++infix];
  }
  if(cSign == '.' || cSign== ',')
  {
   out+='.';
  	cSign=in[++infix];
	 	while(IsDigit(cSign))
	  {
	  	out += cSign;
	   cSign= in[++infix];
	  }
  }
  out += ' ';
 }
 return RET_OK;
}
int parseExpression(String& in, String& out)
{
 int		infix = 0;
 if(parseFormula(in, out, infix))
 {
 	cout << "from parseExpression" << endl;
  return RET_ERR;
 }
 if(infix != in.length())
 {
 	cout << "parseExpression: Unknown symbol c. " << infix +1 << endl;
  return RET_ERR;
 }
 return RET_OK;
}
```

