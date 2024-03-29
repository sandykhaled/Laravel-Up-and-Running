**$user->contacts Vs $user->contacts()**
```
// Accessing the contacts relationship and applying additional constraints
$contacts = $user->contacts()->where('status', 'active')->get();
```
```
// Accessing the contacts relationship directly
$firstContact = $user->contacts; 
```
________________
