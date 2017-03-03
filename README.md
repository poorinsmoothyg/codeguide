# Don't Repeat Yourself

Don’t repeat yourself design principle is about abstracting out common code and putting it in a single location.
It discourages repetitive code. It is about putting one requirement at one piece only. 
It helps us avoid maintenance jargon. Let us see Don’t repeat yourself principle in an example.
 
'''public class Mechanic { 
	public void serviceCar() { 
		System.out.println("servicing car now"); 
	} 
	public void serviceBike() { 
		System.out.println("servicing bike now"); 
	} 
}'''

Notice that there are two methods serviceCar() and serviceBike().
The mechanic services bikes according to their own method. Nothing strange about it.
Now consider the workshop is offering you other services, the mechanic will wash your bike and then service, 
he is also offering to polish vehicles in the service itself.
Now we have to update the code for serviceCar() and serviceBike() also

	'''public void serviceCar() { 
		// washing vehicle here
		System.out.println("servicing car now");
		// polishing vehicle here
	} 
	public void serviceBike() { 
		// washing vehicle here
		System.out.println("servicing bike now");
		// polishing vehicle here
	}'''

Now what is the problem here? Whenever some procedure changes, methods serviceCar and serviceBike also change. 
There is code duplication, one piece of code is repeating for same purpose. 
Here comes the application of don’t repeat yourself principle. It states to abstract out the code that is being duplicated.
So we can write a separate method that performs the tasks which mechanic offers other than servicing.

'''public class Mechanic { 
	public void serviceCar() { 
		System.out.println("servicing car now");
		performOtherTasks();
	} 
	public void serviceBike() { 
		System.out.println("servicing bike now");
		performOtherTasks();
	} 
	public void performOtherTasks() { 
		// do washing here
		// or do something else
		System.out.println("performing tasks other than servicing");
		// do whatever you want to do in the servicing package
	} 
}'''

Now you have created a method performOtherTasks() by applying don’t repeat yourself principle.  
serviceCar() and serviceBike() simply call it. Now whatever changes the workshop offers in service package, 
they can be included in the same method. That code need not replicate in serviceCar() and serviceBike(). 
Thus it makes code more cohesive and maintainable.

Exercise: Ex01

reference: http://www.thejavageek.com/2015/04/10/dont-repeat-yourself-principle/

=============================================================================================================================================
# One Thing: Extract till you Drop.

The following advice has appeared in one form or another for 30 years or more:
"Functions should do one thing. They should do it well. They should do it only."
If you look at the example class at the end of this post, you will see that each function falls into a specific LEVEL from within 
a hierarchy of abstraction. Obviously a function contains a number of steps of functionality, so it is literally 
“doing more than one thing”. However, the rule stated here really means that a function should only do those things that are ONE level
below the stated named of the function — i.e., it should not include calls to functions at other levels of abstraction.
Another way to know that a function is “doing more than one thing” is if you can extract another function from it with a name 
that is not merely a restatement of its implementation.

Consider this class:

  '''class SymbolReplacer {
    protected String stringToReplace;
    protected List<String> alreadyReplaced = new ArrayList<String>();

    SymbolReplacer(String s) {
      this.stringToReplace = s;
    }

    String replace() {
      Pattern symbolPattern = Pattern.compile("\\$([a-zA-Z]\\w*)");
      Matcher symbolMatcher = symbolPattern.matcher(stringToReplace);
      while (symbolMatcher.find()) {
        String symbolName = symbolMatcher.group(1);
        if (getSymbol(symbolName) != null && !alreadyReplaced.contains(symbolName)) {
          alreadyReplaced.add(symbolName);
          stringToReplace = stringToReplace.replace("$" + symbolName, translate(symbolName));
        }
      }
      return stringToReplace;
    }

    protected String translate(String symbolName) {
      return getSymbol(symbolName);
    }
  }'''

And see how about this one 

  '''class SymbolReplacer {
    protected String stringToReplace;
    protected List<String> alreadyReplaced = new ArrayList<String>();
    private Matcher symbolMatcher;
    private final Pattern symbolPattern = Pattern.compile("\\$([a-zA-Z]\\w*)");

    SymbolReplacer(String s) {
      this.stringToReplace = s;
      symbolMatcher = symbolPattern.matcher(s);
    }

    String replace() {
      replaceAllSymbols();
      return stringToReplace;
    }

    private void replaceAllSymbols() {
      for (String symbolName = nextSymbol(); symbolName != null; symbolName = nextSymbol())
        replaceAllInstances(symbolName);
    }

    private String nextSymbol() {
      return symbolMatcher.find() ? symbolMatcher.group(1) : null;
    }

    private void replaceAllInstances(String symbolName) {
      if (shouldReplaceSymbol(symbolName))
        replaceSymbol(symbolName);
    }

    private boolean shouldReplaceSymbol(String symbolName) {
      return getSymbol(symbolName) != null && !alreadyReplaced.contains(symbolName);
    }

    private void replaceSymbol(String symbolName) {
      alreadyReplaced.add(symbolName);
      stringToReplace = stringToReplace.replace(
        symbolExpression(symbolName), 
        translate(symbolName));
    }

    private String symbolExpression(String symbolName) {
      return "$" + symbolName;
    }

    protected String translate(String symbolName) {
      return getSymbol(symbolName);
    }
  }'''

Well, I think it’s pretty clear that each of these functions is doing one thing. 
I’m not sure how I’d extract anything further from any of them

Exercise : Ex02

reference : https://sites.google.com/site/unclebobconsultingllc/one-thing-extract-till-you-drop
	    http://www.itiseezee.com/?p=119
