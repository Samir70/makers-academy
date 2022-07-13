Hello (student_name)

I like how you have listed the main features of the requirements in a user story, and your Account class implements these features.

## Using the app in the IRB

I did the following:
```
> money = Account.new
> money.deposit(28.4556)
> money.deposit(0.04)
> money.statement
date || credit || debit || balance
12/07/2022 || 0.04 ||  || 28.5
12/07/2022 || 28.46 ||  || 28.46
```

Your app accepted the large number of decimals and rounded the first amount to two decimal places. But my balance now has only one decimal place, 28.5, which is unconventional. In the task requirements, the table they provide has two decimal places even when there is zero pence. I would aim for this, and include a test to check for it.

While we are here: If I make a further deposit of 50p, do you know why the balance will be 29.0 rather than 29? 

## Is this app what the client wants?

You have made decisions based on what you think should happen. Maybe the client wants a deposit like 28.4556 to be denied or flagged. But they didn't think of the situation when they sent you the requirements. 

Similarly:
```
> money.withdraw(1000)
=> "insufficient funds"
```

I can't get overdrawn! This seems reasonable, but wasn't asked for in the requirments. You should check with the client rather than assume what they want.

## coding style

These things may not change how well your program works, but they can make it harder to work out **how** it works.

When I first made the account, I got this:
```
> money = Account.new
=>
#<Account:0x0000015aa5e44c50
```
Which doesn't tell me much about what has happened, but it is the default behaviour. I know a new account has been created, but nothing about the account. Maybe I can print it!
```
> puts money
#<Account:0x0000016fb476cc88>
=> nil
```
That didn't help. It is also the default behaviour.

If I am in the IRB and investigating how my class is working, I need some way of seeing what the class is. My only option here is the .statement method. This class isn't too big, so maybe that is sufficient in this case. But can you tell me how you would change both of those default behaviours?

## coding style 2
Why is the Statement class named that way? It doesn't provide the whole statement. It provides only one transaction which will appear on a statement. 
And when it does this, it time stamps it using its own call to the date. So what is TIME (defined on line 5 of account.rb) used for? 

On lines 33 and 38 when using ``` update = @statement.new(...)```

Statement isn't capitalised on those lines because you're not calling the class directly. Instead: you have set a lowercase variable to point to the class. Why you have done things this way? 

I made some changes and the program still works:

```
def initialize(balance: 0)                     # removed: , statement: Statement)
  @balance = balance
  @transactions = []
  # @statement = statement                     # commented out
  @irb_topper = "date || credit || debit || balance"
end

def deposit_update(credit: nil, balance: nil)
  update = Statement.new(credit: credit, balance: balance)    # changed
  @transactions.unshift(update)
end

def withdrawal_update(debit: nil, balance: nil)
  update = Statement.new(debit: debit, balance: balance)      # changed
  @transactions.unshift(update)
end
```

## Which is best at a larger scale?
Finally: I notice that you use .unshift when adding to the transaction list. How does this compare to .push? I had some expections, but did some googling. Now I'm not so sure. Can you do a bit of reasearch on this please?

## Conclusion
Overall, this app is very useable and needs only a few adjustments to meet all requirements. Do also pay attention to how you and other coders may need to interact with your code, especially as it gets larger and more complex.

Overall: Well done.
