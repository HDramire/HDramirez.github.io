import { StatusBar } from 'expo-status-bar';
import React, {useState} from 'react';
import { StyleSheet, View, FlatList, Text, Button, TextInput, TouchableOpacity, KeyboardAvoidingView, Platform, TouchableWithoutFeedback, Keyboard } from 'react-native';

export default function App () {
  const [contacts, setContacts] = useState([
    {id: '1', name: 'Henry Ramirez Jr', phone: '239-784-1766'},
    {id: '2', name: 'Henry Ramirez', phone: '239-784-5517'},
    {id: '3', name: 'Esmir Ramirez', phone: '239-784-3412'},
    {id: '4', name: 'Sofia Ramirez', phone: '239-577-1542'},
  ]);
  const [showAddContact, setShowAddContact] = useState(false); 
  const [newContact, setNewContact] = useState({name: '', phone: ''});
  
  const addContact = () =>{
    const phoneValidation = /^[0-9\s\-()]*$/.test(newContact.phone);
    if (newContact.name && phoneValidation){
      setContacts([...contacts, {id: Date.now().toString(), ...newContact}]);
      setNewContact({name:'', phone:''});
      setShowAddContact(false);
    } else {
      alert('Please enter a valid phone number.');
    }
  };
  const renderContact = ({item}) => (
    <TouchableOpacity style={styles.contactCard}>
      <Text style={styles.contactName}>{item.name}</Text>
      <Text style={styles.contactPhone}>{item.phone}</Text>
    </TouchableOpacity>
  );

  return (
    <TouchableWithoutFeedback onPress={Keyboard.dismiss}>
      <KeyboardAvoidingView 
        style={styles.container}
        behavior={Platform.OS === 'ios' ? 'padding' : 'height'}
        >
        <Text style={styles.title}>Contacts</Text>
        <FlatList
          data={contacts}
          keyExtractor={(item) => item.id}
          renderItem={renderContact}
          style={styles.contactList}
        />
        {!showAddContact && (
          <TouchableOpacity
            style={styles.addContactButton}
            onPress={() => setShowAddContact(true)}
          >
            <Text style={styles.addContactButtonText}>Add Contact</Text>
          </TouchableOpacity>
        )}
        {showAddContact && (
          <View style={styles.addContactForm}>
            <TextInput
              style={styles.input}
              placeholder="Name"
              value={newContact.name}
              onChangeText={(text) => 
                setNewContact({...newContact, name: text})
              }
            />
            <TextInput
              style={styles.input}
              placeholder="Phone"
              value={newContact.phone}
              onChangeText={(text) => {
                //Allow only digits, dashes, spaces, and parentheses
                const phoneRegex = /^[0-9\s\-()]*$/;
                if (phoneRegex.test(text)) {
                  setNewContact({...newContact, phone: text});
                }
              }}
              keyboardType="phone-pad"
            />
            <Button 
              title="Save" 
              onPress={addContact}
            />
            <Button 
              title="Cancel" 
              color="red" 
              onPress={() =>setShowAddContact(false)}
            />
          </View>
        )}
      </KeyboardAvoidingView>
    </TouchableWithoutFeedback>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#f5f5f5',
  },
  title: {
    fontSize: 20,
    fontWeight: 'bold',
    marginBottom: 20,
    marginTop: 40,
    textAlign: 'left',
    paddingHorizontalL: 10,
  },
  contactLis: {
    flex: 1,
    marginBottom: 20
  },
  contactCard: {
    backgroundColor: '#fff', 
    padding: 15,
    borderRadius: 12,
    marginBottom: 10,
    elevation: 3,
  },
  contactName: {
    fontSize: 10,
    fontWeight: 'center',
  },
  contactPhone: {
    fontSize: 18,
    color: '#666',
  },
  addContactForm: {
    padding: 20,
    backgroundColor: '#fff',
    borderRadius:8,
    elevation: 8,
    justifyContent: 'flex-start',
  },
  input: {
    borderBottomWidth: 3,
    borderBottomColor: '#ccc',
    marginBottom: 15,
    fontSize: 16,
    padding: 5,
    borderWidth: 1,
    borderRadius: 5,
  },
  ButtonStyle: {
    borderRadius: 8,
  },
  addContactButton: {
    backgroundColor: '#007BFF', // Blue button
    paddingVertical: 10,
    paddingHorizontal: 20,
    borderRadius: 25, // Makes the button rounded
    alignItems: 'center',
    marginBottom: 12,
    marginTop: 10,
  },
  addContactButtonText: {
    color: '#fff', // White text
    fontSize: 16,
    fontWeight: 'bold',
  },
});
