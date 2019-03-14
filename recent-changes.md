# Recent Changes
#### Replacing the using of the Checks class with the CheckSave module where the transaction screen is concerned.

Saving a check with the default check name.
```ruby
@transaction_screen.checks.save_check_with_prefilled_name  # old

@transaction_screen.save_new_check  # new
```
Saving and naming a check.
```ruby
@transaction_screen.checks.save_and_name_check('Check Name')  # old

@transaction_screen.save_new_check('Check Name')  # new
```
`checkname` is an optional parameter
```ruby
def save_new_check(checkname = nil)
  click_save
  name_check(checkname) unless checkname.nil?
  complete_saving_check
end
```

#### PaymentMethod class replacing some of the functunality currently provided by the SaleTotal class
This is part of making our classes and modules more focussed. Their functionality should be obvious.

```ruby
# old
@transaction_screen.sale_total.pay_cash
@transaction_screen.sale_total.pay_split_tender

# new
@transaction_screen.payment_method.pay_cash
@transaction_screen.payment_method.pay_split_tender
```
If in doubt, have a look at the PaymentMethod class. Chances are if the methods is there, you should probably be using that in place of the SaleTotal equivilent.
