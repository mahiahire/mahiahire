- 👋 Hi, I’m @mahiahire
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
mahiahire/mahiahire is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->


// React Native Example for a Simple Dating App

import React, { useState, useEffect } from 'react';
import { StyleSheet, Text, View, TextInput, Button, FlatList, Image } from 'react-native';
import { launchCamera, launchImageLibrary } from 'react-native-image-picker';

const App = () => {
  const [users, setUsers] = useState([]);
  const [username, setUsername] = useState('');
  const [bio, setBio] = useState('');
  const [photo, setPhoto] = useState(null);

  useEffect(() => {
    // Fetch users from API (replace with your actual API endpoint)
    fetch('https://your-api-endpoint.com/users')
      .then(response => response.json())
      .then(data => setUsers(data));
  }, []);

  const handleProfileUpdate = () => {
    // Send updated profile information to the server
    const formData = new FormData();
    formData.append('username', username);
    formData.append('bio', bio);
    if (photo) {
      formData.append('photo', {
        uri: photo.uri,
        type: photo.type,
        name: 'profile.jpg', 
      });
    }

    fetch('https://your-api-endpoint.com/profile', {
      method: 'POST',
      body: formData,
    })
      .then(response => response.json())
      .then(data => {
        // Handle success or error
      });
  };

  const handleImagePick = () => {
    const options = {
      mediaType: 'photo',
      maxWidth: 200,
      maxHeight: 200,
    };

    launchImageLibrary(options, (response) => {
      if (!response.didCancel) {
        setPhoto(response.assets[0]);
      }
    });
  };

  const renderUser = ({ item }) => (
    <View style={styles.userCard}>
      {item.photo ? (
        <Image source={{ uri: item.photo }} style={styles.userImage} />
      ) : (
        <View style={styles.userImagePlaceholder} />
      )}
      <Text style={styles.username}>{item.username}</Text>
      <Text style={styles.bio}>{item.bio}</Text>
    </View>
  );

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Dating App</Text>

      <TextInput
        style={styles.input}
        placeholder="Username"
        value={username}
        onChangeText={setUsername}
      />
      <TextInput
        style={styles.input}
        placeholder="Bio"
        value={bio}
        onChangeText={setBio}
      />

      <Button title="Choose Photo" onPress={handleImagePick} />
      {photo && <Image source={{ uri: photo.uri }} style={styles.previewImage} />}

      <Button title="Update Profile" onPress={handleProfileUpdate} />

      <FlatList
        data={users}
        keyExtractor={item => item.id.toString()}
        renderItem={renderUser}
      />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
  },
  input: {
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    marginBottom: 10,
    paddingHorizontal: 10,
  },
  userCard: {
    borderWidth: 1,
    borderColor: 'gray',
    padding: 10,
    marginBottom: 10,
  },
  userImage: {
    width: 100,
    height: 100,
    borderRadius: 50,
  },
  userImagePlaceholder: {
    width: 100,
    height: 100,
    borderRadius: 50,
    backgroundColor: '#ccc',
    justifyContent: 'center',
    alignItems: 'center',
  },
  username: {
    fontSize: 18,
    fontWeight: 'bold',
    marginTop: 10,
  },
  bio: {
    marginTop: 5,
  },
  previewImage: {
    width: 200,
    height: 200,
    marginBottom: 10,
  },
});

export default App;
