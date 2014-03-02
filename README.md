Autoassign plugin for Pintask
=========================

Add yourself as card member (if there are no other card members) when adding to or dropping it on a list that you are a member of.

This versatile plugin is only a couple lines of code:

```coffee
if !Cards
  throw "Hey, where are my Cards?"
if !Lists
  throw "Look, I need those Lists, too!"

Cards.before.insert (userId, card) ->
  list = Lists.findOne(card.listId)
  if userId in list.memberIds and not card.memberIds.length
    card.memberIds.push(userId)

Cards.before.update (userId, card, fieldNames, modifier, options) ->
  if modifier.$set?.listId
    list = Lists.findOne(modifier.$set.listId)
    if userId in list.memberIds and not card.memberIds.length
      if not modifier.$addToSet?.memberIds # don't add yourself when adding another member
        modifier.$addToSet = {memberIds: userId}
```

So easy to make your own plugin ;)
