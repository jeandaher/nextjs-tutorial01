## Hello Component 

1. You have 2 variables ***name***, **message** and the methods: **displayMessage**, **updateName**
2. The user enter the name (stored into name)
3. The user click on button **Say Hello**, the system will display the message Hello and the name !!!!

### Create the Component Hello    

Create the logic in the file useHello.ts under the folder hello with the following codes:
```.js
"use client"
import { useState } from 'react'; 

const useHello = () => {
  const [name, setName] = useState<string>('');
  const [message, setMessage] = useState<string>('');

  const updateName = (value: string) => {
    setName(value);
    };

  const displayMessage = () => {
    if (name !== '') {
      setMessage('Hello ' + name + ' !!!');
      setName('');
    }
  };

  return {
    name,
    message,
    updateName,
    displayMessage,
  };
};

export default useHello;
```


Create the XUI in the file Hello.tsx under the folder hello with the following codes:
```.js
"use client"
import React from 'react'; 
import {
  Container,
  Box,
  Heading,
  Input,
  Button,
  VStack,
  Text,
  ChakraProvider, 
} from '@chakra-ui/react';
import theme from '@/themes/chakra-theme'; // Import your custom theme
import useHello from './useHello';

const Hello = () => {

  const {
    name,
    message,
    updateName,
    displayMessage,
  } = useHello();
  
  return (
    <ChakraProvider theme={theme}> 
      <Container maxWidth="600px" margin="0 auto" padding="20px">
        <VStack spacing="20px">
          <Heading fontSize="2.5rem" fontWeight="bold" color="brand.500"> {/* Use the custom color */}
            Hello
          </Heading>
          <Box display="flex" alignItems="center">
            
            <Input
              fontSize="1.2rem"
              padding="10px"
              marginRight="10px"
              flexGrow="1"
              borderRadius="4px"
              borderColor="#ccc"
              placeholder="Enter your name..."
              value={name}
              onChange={(e) => updateName(e.target.value)}
            />
            <Button
              fontSize="1.2rem"
              padding="10px 20px"
              backgroundColor="brand.500"
              color="white"
              borderRadius="8px"
              cursor="pointer"
              onClick={displayMessage}
            >
              Say Hello
            </Button>
          </Box>
            <Text textAlign="center" fontSize="1.2rem" color="#777">
              {message}
            </Text>
        </VStack>
      </Container>
    </ChakraProvider>
  );
};

export default Hello;
```


Modify the content of the app/page.tsx to include the Hello component as the following codes:
```.js
import Hello from "@/components/hello/Hello"

const Home = () => {
	return (
		<Hello />
	);
};

export default Home;
```


Build and run the application:
```.js
import Hello from "@/components/hello/Hello"

const Home = () => {
	return (
		<Hello />
	);
};

export default Home;
```



