# Debug log

Your notes. One entry per bug you fixed, using the template below.

This file is read as carefully as your code. A correct fix you cannot
explain counts for little; a bug you could not fix but investigated
honestly still counts for something.



## CC-02 — "<Can't read anything in dark mode>"

**Reproduced:** while switching in dark mode the dish name and text becomes unreadable.

**Cause:** because in dish_body{colour:#2b2118} which is defined for both light and dark backgrounds. but text colour should be changed according to background theme

**Fix:** in the css we define different colours for light and dark background. colour: var(--ink) so if theme=dark then colour selected is from dark backgronk --ink we had defined.

**Checked:** fixed

**Time:**


### CC-08 — "<An old coupon still works>"

**What I tried:** **reproduce** for an old coupon applied i applied freshers24 and it still got applied giving discount

**cause** for an old coupon in the apply coupon dates are not compared. if todays date is more than expiry date then coupon should not be applied . so condition is checked if nowdate()>expiry date expired coupon should be printed in the applycopoun function 
function applyCoupon(code, lines, coupons, { slot = null, now = new Date() } = {}) {
  if (!code) return { valid: false, discount: 0, reason: null };
in this function expiry date and current date are not compared.

**fixed** if (coupon.expiresAt && new Date(now) > new Date(coupon.expiresAt)) {
  return { valid: false, discount: 0, reason: 'Coupon has expired' };
  }  in pricing attribute where only discount is changed to 0 if condition is satisfied.Condition checked and discount made=0.

  **fixed** yes fixed the bug

**Where I got to:** in logic pricing i had checked the condition and if condition is checked the coupon has expired is printed. freshers24 was the coupon with expiry date last year still usable means condition for checking nowdate() and expiry date is not done.
fixed the bug


## CC-02 — "<I ordered more than they had>"

**Reproduced:** if in stock only 3 samosas are left but we ordered added the items qunatity as 5 and even if quantity is greater then stock still we are able to order it.

**Cause:** in our cart.js condition was not checked if in our added cart quantity is greater than stock. if quantity is greater then stock and button pressed is increment then warning is displayed by toast("warn") that order is invalid and return is passed next so that next operations are not performed.

**Fix:** delegate(cartItemsBox, '[data-cart]', 'click', (e, node) in this function it deals with interactions of buttons increment decrement  if (action === 'remove') {actions like decrement and increment so in increment else statement putted condition  then warning is displayed as toast.
 if (action === 'inc' && cartQty(dishId) >= Number(dish.stock)) {
    toast('Item not available: maximum stock reached', 'warn');
    return;
  }
  condition specified for quantity>=stock
  in the cart cart.js i had made changes adding in cart becomes disable after condition reached and quantity greater than stock warning is shown.

**Checked:** fixed

**fixed** in our cart we cannot order more than the stock and message is displayed inside the cart and also order is also not booked no next functions are performend if quantity>=stock condition is met.
"+ button should also be disabled when quantity is reached upto stock but unable to disable + button" made chances such as maxreached =stock and when quantity reaches maxreaches condition becomes false and we disable the button it is core logic behind disabling the button. 

**Time:**



## Could not fix

For anything you investigated but did not solve. Say what you tried and
where you got to. This is worth marks — leaving it blank when you got
stuck is not.


### CC-07: "Cancelling makes it worse"

**What I tried:** **reproduce** if i ordered samosa and cancelled so the quantity should be added in stock as order has not been place. 6 stocks of samosas left 2 ordered and order cancelled but the stock shows 4 samosa even if i had cancelled 2 samosas.
when ordered any item and cancelled so stock should be refilled by the quantity that we had ordered as we had cancelled our order so stock items should not be affected as order is not placed,

**cause** because in backend logic validate.js there are function like taking stock off when the order is place 

* Take stock off the menu for a confirmed order.
 * Re-checks every line first, so two requests cannot take the same last plate.
 */
function reserveStock(items, menu) {
  const errors = [];
  const next = menu.map((d) => ({ ...d }));

  items.forEach((item) => {
    const dish = next.find((d) => d.id === Number(item.dishId));
    if (!dish) return errors.push(`dish ${item.dishId} vanished`);
    dish.stock -= item.qty;

    stock is equal to stock - quantity

    similarly if order is cancelled then stock should be update by stock=stock+quantity but originally the function had condition of subtraction(stock=stock-quantity ) only
    function releaseStock(items, menu) {
  return menu.map((dish) => {
    const line = items.find((i) => Number(i.dishId) === dish.id);
    if (!line) return dish;
    return { ...dish, stock: dish.stock - line.qty };
  });
}

**fixed** function releaseStock(items, menu) {
  return menu.map((dish) => {
    const line = items.find((i) => Number(i.dishId) === dish.id);
    if (!line) return dish;
    return { ...dish, stock: dish.stock + line.qty };
  });
}

stock=stock+quantity logic required for refilling stock
  **fixed** yes fixed the bug



### CC-miscellaneous — "when the coupon is appied still the value of coupon user does not decrease and 501 users left is constant"

**Reproduced:** if coupon byte10 is applied so after applying the value should be decreased and usersleft count should be reduced but constant count is maintained.

**Cause:** Because when coupon is applied in applycoupon() in last if coupon is applied so all conditions are checked if coupon is valid or not but when coupon is applied then user quantity is not affected or altered but if coupon is applied 1 user should be subtracted from couponsuserid.
"no condition for couponuser.id-=1 if coupon is applied"

**Fix:** coupons.userleft-=1; in the apply coupon() function inside pricing in logic backend where after coupon is applied befor last condition this condition is fullfilled and after applying coupon our couponusersleft are reduced by 1.

**Checked:** fixed

**Time:** about 40 minutes, most of it working out that the cart and the
order screen round in different places.


## Extra credit

Anything not on the bug log: a problem you found yourself, a test you
wrote, or a fix you are unsure about. Same format, plus one line on how
you noticed it.
