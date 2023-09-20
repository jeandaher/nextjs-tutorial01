# NextJs Tutorial 01 - ToDo    

In this tutorial, we will learn NextJs and Applying CRUDE methods on ToDo Object.    

Next.js is a a good Framework based on React and that e­nables server-side­ rendering and enhance­s the developme­nt of interactive user inte­rfaces.     

1. Create the Application    
```.js
npx create-next-app my-first-app
```
Options    
√ Would you like to use TypeScript? ... No / *Yes*    
√ Would you like to use ESLint? ... No / *Yes*    
√ Would you like to use Tailwind CSS? ... *No* / Yes    
√ Would you like to use `src/` directory? ... No / *Yes*    
√ Would you like to use App Router? (recommended) ... No / *Yes*    
√ Would you like to customize the default import alias? ... *No* / Yes    


Go to application folder, and open it using vscode editor:    
```.js
cd my-first-app
code .    
```

To install Chakra ui :
```.js
npm install @chakra-ui/react @emotion/react @emotion/styled framer-motion
```

Under src create 4 folders :
```.js
components
themes
todo
utils
```

Add new file themes\chakra-theme.tsx with the following content:    
```.js
"use client"
import { extendTheme } from '@chakra-ui/react';

const theme = extendTheme({
  colors: {
    brand: {
      500: '#4caf50', // Define your custom color
    },
  },
  fonts: {
    body: 'Arial, sans-serif', // Define your custom font
  },
});

export default theme;
```

Add the utility file in utils/formatting.ts with the following content:    
```.js
export const getFormattedDateTime = (): string => {
    const now = new Date();
    const year = now.getFullYear().toString(); 
    const month = String(now.getMonth() + 1).padStart(2, '0'); // Months are 0-based
    const day = String(now.getDate()).padStart(2, '0');
    const hours = String(now.getHours()).padStart(2, '0');
    const minutes = String(now.getMinutes()).padStart(2, '0');
    const seconds = String(now.getSeconds()).padStart(2, '0');
    return `${year}${month}${day}${hours}${minutes}${seconds}`;
  }
```

Add the logic of todo in the **hook** file todo\useTodoList.ts with the following content:    
```.js
"use client"
import { useState } from 'react';
import { getFormattedDateTime } from '@/utils/formatting';

export interface RecordType {
  id: string;
  value: string;
}

const useTodoList = () => {
  const [valueInput, setValueInput] = useState<string>('');
  const [list, setList] = useState<RecordType[]>([]);

  // Set a value input value
  const updateItem = (value: string) => {
    setValueInput(value);
  };

  // Add item if value input is not empty
  const addItem = () => {
    if (valueInput !== '') {
      const valueInputItem = {
        id: getFormattedDateTime(),
        value: valueInput,
      } as RecordType;

      // Update list
      setList([...list, valueInputItem]);

      // Reset state
      setValueInput('');
    }
  };

  // Function to delete item from list using id to delete
  const deleteItem = (id: string) => {
    const updatedList = list.filter((item) => item.id !== id);
    setList(updatedList);
  };

  const editItem = (index: number) => {
    const editedTodo = prompt('Edit the todo:');
    if (editedTodo !== null && editedTodo.trim() !== '') {
      const updatedTodos = [...list];
      updatedTodos[index].value = editedTodo;
      setList(updatedTodos);
    }
  };

  return {
    valueInput,
    list,
    addItem,
    updateItem,
    deleteItem,
    editItem,
  };
};

export default useTodoList;
```


Add the User Interface of todo in file todo\TodoList.tsx with the following content:    
```.js
"use client"
import React from 'react';
import useTodoList from './useTodoList';
import {
  Container,
  Box,
  Heading,
  Input,
  Button,
  VStack,
  Text,
  ChakraProvider, // Import ChakraProvider
} from '@chakra-ui/react';
import theme from '@/themes/chakra-theme'; // Import your custom theme

const TodoList = () => {
  const {
    valueInput,
    list,
    addItem,
    updateItem,
    deleteItem,
    editItem,
  } = useTodoList();

  return (
    <ChakraProvider theme={theme}> {/* Wrap your component in ChakraProvider with your custom theme */}
      <Container maxWidth="600px" margin="0 auto" padding="20px">
        <VStack spacing="20px">
          <Heading fontSize="2.5rem" fontWeight="bold" color="brand.500"> {/* Use the custom color */}
            To Do List
          </Heading>
          <Box display="flex" alignItems="center">
            <Input
              fontSize="1.2rem"
              padding="10px"
              marginRight="10px"
              flexGrow="1"
              borderRadius="4px"
              borderColor="#ccc"
              placeholder="Add item..."
              value={valueInput}
              onChange={(e) => updateItem(e.target.value)}
            />
            <Button
              fontSize="1.2rem"
              padding="10px 20px"
              backgroundColor="brand.500"
              color="white"
              borderRadius="8px"
              cursor="pointer"
              onClick={addItem}
            >
              ADD
            </Button>
          </Box>
          <Box
            background="#f9f9f9"
            padding="20px"
            borderRadius="8px"
            width="100%"
          >
           {list.length > 0 ? (
            list.map((item, index) => (
              <Box
                key={index}
                display="flex"
                justifyContent="space-between"
                alignItems="center"
                marginBottom="10px"
              >
                <Text fontSize="1.2rem" flexGrow="1">
                  {item.value}
                </Text>
                <Box>
                  <Button
                    padding="10px"
                    backgroundColor="#f44336"
                    color="white"
                    borderRadius="8px"
                    cursor="pointer"
                    marginRight="10px"
                    onClick={() => deleteItem(item.id)}
                  >
                    Delete
                  </Button>
                  <Button
                    padding="10px"
                    backgroundColor="#2196f3"
                    color="white"
                    borderRadius="8px"
                    cursor="pointer"
                    onClick={() => editItem(index)}
                  >
                    Edit
                  </Button>
                </Box>
              </Box>
            ))
          ) : (
            <Text textAlign="center" fontSize="1.2rem" color="#777">
              No items in the list
            </Text>
          )}
          </Box>
        </VStack>
      </Container>
    </ChakraProvider>
  );
};

export default TodoList;
```


Replace the content of page.tsx by the following:    
```.js
import TodoList from "@/components/todo/TodoList"
const Home = () => {
	
	return (
		<TodoList />
	);
};

export default Home;
```
Build and Run your application    
```.js
npm run build
npm run start
```
