# Christoffels Kitchen
import React, { useState } from 'react';
import { View, Text, TextInput, Button, FlatList, StyleSheet } from 'react-native';
import { Picker } from '@react-native-picker/picker';

export default function App() {
  const [dishName, setDishName] = useState('');
  const [description, setDescription] = useState('');
  const [course, setCourse] = useState('Starters');
  const [price, setPrice] = useState('');
  const [menuItems, setMenuItems] = useState([
    // --- Preloaded Starters ---
    { id: '1', dishName: 'Chicken livers with creamy garlic sauce with ciabatta bread', description: 'Tender livers cooked in rich creamy garlic sauce with a hint of spice. With a warm, toasted ciabatta bread.', course: 'Starters', price: 150 },
    { id: '2', dishName: 'California Rolls (8)', description: 'Inside-out seaweed wrap, crabstick, avocado, cucumber and sesame seed.', course: 'Starters', price: 105 },
    { id: '3', dishName: 'Mussels (6)', description: 'Fresh mussels served in a velvety garlic and cream sauce infused with parsley and a touch of white wine.', course: 'Starters', price: 220 },

    // --- Preloaded Mains ---
    { id: '4', dishName: 'Creamy mushroom prawn pasta', description: 'Creamy pasta with juicy prawns, sautéed mushrooms, garlic, and a rich parmesan cream sauce.', course: 'Main', price: 205 },
    { id: '5', dishName: 'Fried Rice with Buttercream Chicken', description: 'Savoury stir-fried rice served with tender chicken in a rich, buttery cream sauce.', course: 'Main', price: 150 },
    { id: '6', dishName: 'Double Beef Burger with Cheese', description: 'Two juicy beef patties with double cheddar cheese, fresh toppings, and crispy chips on the side.', course: 'Main', price: 185 },
    { id: '7', dishName: 'Rump Steak with Chips or Vegetables', description: 'Tender grilled rump steak served with crispy chips or fresh seasonal vegetables.', course: 'Main', price: 280 },

    // --- Preloaded Desserts ---
    { id: '8', dishName: 'Malva Pudding with Homemade Custard', description: 'Warm, caramel-soaked sponge pudding served with smooth, creamy custard.', course: 'Dessert', price: 70 },
    { id: '9', dishName: 'Strawberry Cheesecake', description: 'Creamy cheesecake on a buttery crust topped with fresh strawberry sauce.', course: 'Dessert', price: 65 },
    { id: '10', dishName: 'Peppermint Crisp Cake', description: 'Creamy caramel and whipped filling layered with Peppermint Crisp chocolate on a soft biscuit base.', course: 'Dessert', price: 75 },
    { id: '11', dishName: 'Chocolate Mousse', description: 'Light and velvety dark chocolate mousse with a rich, creamy finish.', course: 'Dessert', price: 50 },
    { id: '12', dishName: 'Red Velvet Cake', description: 'Moist red velvet cake layered with creamy, tangy cream cheese frosting.', course: 'Dessert', price: 65 },
  ]);

  const courses = ['Starters', 'Main', 'Dessert'];

  const addMenuItem = () => {
    if (dishName && description && price) {
      const newItem = { id: Date.now().toString(), dishName, description, course, price: parseFloat(price) };
      setMenuItems([...menuItems, newItem]);
      setDishName('');
      setDescription('');
      setPrice('');
      setCourse('Starters');
    }
  };

  // Calculate total cost
  const totalCost = menuItems.reduce((sum, item) => sum + item.price, 0);

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Chef Menu App</Text>

      {/* Input Section */}
      <TextInput
        style={styles.input}
        placeholder="Dish Name"
        value={dishName}
        onChangeText={setDishName}
      />
      <TextInput
        style={styles.input}
        placeholder="Description"
        value={description}
        onChangeText={setDescription}
      />
      <Picker
        selectedValue={course}
        style={styles.input}
        onValueChange={(itemValue) => setCourse(itemValue)}
      >
        {courses.map((c) => (
          <Picker.Item key={c} label={c} value={c} />
        ))}
      </Picker>
      <TextInput
        style={styles.input}
        placeholder="Price"
        keyboardType="numeric"
        value={price}
        onChangeText={setPrice}
      />
      <Button title="Add Menu Item" onPress={addMenuItem} />

      {/* Home Screen */}
      <Text style={styles.subtitle}>Menu Items ({menuItems.length})</Text>
      <FlatList
        data={menuItems}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View style={styles.item}>
            <Text style={styles.itemText}>{item.dishName} - {item.course}</Text>
            <Text>{item.description}</Text>
            <Text>Price: R{item.price.toFixed(2)}</Text>
          </View>
        )}
      />

      {/* Total Cost */}
      <Text style={styles.total}>Total Cost: R{totalCost.toFixed(2)}</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 20, marginTop: 40 },
  title: { fontSize: 24, fontWeight: 'bold', marginBottom: 20 },
  subtitle: { fontSize: 18, marginVertical: 10 },
  input: { borderWidth: 1, borderColor: '#ccc', padding: 10, marginVertical: 5 },
  item: { padding: 10, borderBottomWidth: 1, borderBottomColor: '#ddd' },
  itemText: { fontWeight: 'bold' },
  total: { fontSize: 20, fontWeight: 'bold', marginTop: 15, color: '#2a9d8f' },
});

