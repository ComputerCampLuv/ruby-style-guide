# ruby-style-guide

### Recent Changes
###### Replacing the using of the Checks class with the CheckSave module where the transaction screen is concerned.
Saving a check with the default check name.
```
@transaction_screen.checks.save_check_with_prefilled_name  # old

@transaction_screen.save_new_check  # new
```
Saving and naming a check.
```
@transaction_screen.checks.save_and_name_check('Check Name')  # old

@transaction_screen.save_new_check('Check Name')  # new
```
`checkname` is an optional parameter
```
def save_new_check(checkname = nil)
  click_save
  name_check(checkname) unless checkname.nil?
  complete_saving_check
end
```

###### PaymentMethod class replacing some of the functunality currently provided by the SaleTotal class
This is part of making our classes and modules more focussed. Their functionality should be obvious.
```
# old
@transaction_screen.sale_total.pay_cash
@transaction_screen.sale_total.pay_split_tender

# new
@transaction_screen.payment_method.pay_cash
@transaction_screen.payment_method.pay_split_tender
```
If in doubt, have a look at the PaymentMethod class. Chances are if the methods is there, you should probably be using that in place of the SaleTotal equivilent.

###### Blocks
Single line blocks wrapped in curly braces with single spacing either end
```
2.times { @transaction_screen.transaction_grid.add_generic_item }
```
Multi line blocks wrapped in a `do ... end`
```
2.times do
  @transaction_screen.transaction_grid.add_generic_item
  @transaction_screen.save_new_check
end
```

### The dumping ground
need to agree on usage, what is worth pulling out as helper methods.
are these local or global
```
RSpec.describe 'example template' do
  let(:cashier_passcode)  { '3456' }
  let(:manager_passcode)  { @appium_config.manager_code }
  let(:gift_card)         { @appium_config.gift_card_id_1 }
```
I'll put this here but it's not exactly relevant yet
```
# this will be useful to us at some point in the future.
# good to have it clear in your head what happends and in what order
# and this doesn't take into consideration the that the same hooks can be used multiple times
# so nested contexts could provide you with some unexpected behaviour if you're not careful
# to the best of my knowledge `around :all` has been deprecated
# I'm not sure if `around :suite` is a thing!

before :suite {...} 
  
before :all {...}
    
around :each do
      
  before :each {...}
        
  example.run
          
  after :each {...}

end
    
after :all {...}

after :suite {...}

end
```
Dumping this here for the time being. I will clean this up
```
before :each do
    receipt_settings.on_screen_tips_enabled
    BrowserlessBo::Endpoints::Bo::ReceiptSettings.update(bo_context, receipt_settings) # seperate actions that are not related
      
    @bo_actions.select_register_type('Restaurant')
    # get and set are conventions for instance variables however this isn't a ruby convention
    # we have a decision to make on our own convention here
    # guest_count_prompt?
    # guest_count_prompt=
    @bo_actions.set_guest_count_prompt(false)

    @shift_screen.open_shift.sidebar.close # @shift_screen.open_shift('100.00').sidebar.close
  end
  
  after :each do 
    @bo_actions.select_register_type('Retail')
    # consider the actions you take in the back office and be sure to reverse any actions that could effect another test.
    # assume that any test may follow this one
  end
    
  it 'will perform said task', :android, :skip_bo do # bevity, should vs will, case_id's?, flags
    @shift_screen.sidebar.sign_out
    # gaps between screen changes
    @signin_screen.sign_in @appium_config.manager_code
    @signin_screen.sign_in '1111'
    @signin_screen.sign_in(cashier_passcode)

    # needless indentation and curly backets used on a multiline block
    3.times { @transaction_screen.stock_item_grid.add_item '$10 Discount Only General'
              @transaction_screen.save_new_check } 
    3.times do 
      @transaction_screen.stock_item_grid.add_item '$10 Discount Only General'
      @transaction_screen.save_new_check
    end 
    @checks_screen.select_checks_and_begin_merge('1', '2') # parentheses

    expect(@checks_screen.does_element_exist?('Check Name Placeholder')).to eql true
    expect(@checks_screen.does_element_exist?('Check Name Placeholder')).to be true
    # be true, eq for everything else. most everything is a string, does this need to be the case

    @checks_screen.name_check 'Custom'
    @checks_screen.complete_merge
    # gaps bewteen 
    expect(@checks_screen.get_check_name '1').to eql 'Custom'
    expect(@checks_screen.check_name('1')).to eq('Custom')

    # grouped as they represent one operation
    # granular is okay because it represents part of the test
    @checks_screen.enter_select_mode
    @checks_screen.select_checks('1','2')
    @checks_screen.open_actions_menu
    @checks_screen.begin_merge

    expect(@checks_screen.does_element_exist?('Check Name Placeholder')).to eql true

    # and used as part of a method name is generally a no go
    # consider merge_checks(*check_indecies)
    @checks_screen.select_checks_and_begin_merge '1', '2'

    # use symbols where possible for keys
    # symbols used here for percent and amount as it highlights the non arbatrary nature of them
    # percent, and amount are the only valid options here.
    # it wont always be clear if ints and symbols will work in place of strings
    suggested_tips = [
      { field: 1, type: :percent, value: 60 },
      { field: 2, type: :amount, value: 10 },
      { field: 3, type: :percent, value: 80 }
    ]

    receipt_settings.set_tips(suggested_tips)
    BrowserlessBo::Endpoints::Bo::ReceiptSettings.update(bo_context, receipt_settings) 

    expect(@shift_screen.correct_menu_options_displayed_to_cashier?).to be true
    # here the expectation is carried out with the page object. which isn't very upfront and doesn't
    # provide great debugging potential
    visible_menu_options = @shift_screen.sidebar.visible_menu_options
    cashier_menu_options = @shift_screen.sidebar.cashier_menu_options
    expect(visible_menu_options).to match_array(cashier_menu_options)
    # actual vs expected compared within the example
    # a failure would inspect both elements and highight discrepancies
    # in place of 'expected true, got false
    expect(@transaction_screen.sale_total.get_total).to eql '8.70'
    # are we happy with the comparrisons to all be on strings, could get total return a float?
    # needless step?!
    expect(@transaction_screen.sale_total.get_total).to eq(8.70)

    # not sure about this expectation, seems like this is more likely to turn a 'broken' test into a 'failing' test
    expect(@split_tenders_payment_screen.sale_complete?).to be true

    @transaction_screen.wait_until_visible 'Split 51'
    # wait for something to be true and then confirm just to be sure!!
    expect(@split_screen.does_element_exist? 'Split 51').to eql true
  
    sign_out_and_sign_back_in '3456'
    # i wouldn't mind helpers returning a variable but I'd rather helper that bundled actions were called on a page object
  end
end
# with all the comments its hard to get spacing across

RSpec.describe '...' do 
  let(:variable) { '...' }
  
  context '...' do 
    it '...' do
      @transaction_screen.do_a_thing
      @transaction_screen.do_another_thing
      @transaction_screen.access_checks

      @checks_screen.do_some_things
      @checks_screen.merge_some_checks
      @checks_screen.access_mergered_check

      @transaction_screen.add_item
      @transaction_screen.pay_cash

      @transaction_screen.add_items('this', 'that')
      @transaction_screen.save_new_check
    end
  end

  context '...' do 
    before :each do 
      [...]
    end

    it '...' do
      [...]
    end
  end
end
```

