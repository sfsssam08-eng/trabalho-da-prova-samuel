//Samuel Ferreira da Silva Santos//
import {useState} from "react";
import {StyleSheet, Text, View,TextInput,Button,ScrollView,Alert} from 'react-native';
import { useNavigation} from '@react-navigation/native';
import {getFirestore,collection,onSnapshot,deleteDoc,doc,addDoc} from 'firebase/firestore'
import {db} from "../components/firebaseConnections";
import {initializeApp} from "firebase/app"
import{getFirestore} from 'firebase/firestore'

import components from ".src copy/src 2/ProvaAPP/components"
import axios from "axios";


export default function UserForms({Navigation}) {
 const navigation= useNavigation();
 
  const [Nome,setNome] = useState('');
const [Letra,setLetra] = useState('');
const [Consoante,setConsoante] = useState('');
const [Substantivo,setSubstantivo] = useState('');
const [Preposiçao,setPreposiçao] = useState('');
const [Adjetivo,setAdjetivo] = useState('');
const [Conjunçao,setConjunçao] = useState('');
  
const [cep, setcep] = useState('');
  const [Logradouro, setLogradouro] = useState('');
  const [Localidade, setLocalidade] = useState('');
  const [Resultado, setResultado] = useState('');

  async function buscarCEP() {
    if (cep.length !== 8) {
      Alert.alert('Erro', 'CEP deve ter 8 dígitos.');
      return false;
    }

    try {
      const response = await fetch(`https://viacep.com.br/ws/${cep}/json/`);
      const data = await response.json();

      if (data.erro) {
        Alert.alert('Erro', 'CEP não encontrado.');
        return false;
      }

      setLogradouro(data.logradouro || '');
      setLocalidade(data.localidade || '');
      setResultado('CEP encontrado com sucesso');
      return true;

    } catch (error) {
      Alert.alert('Erro', 'Erro ao buscar o CEP.');
      return false;
    }
  };

  const enviarDados = async () => {
    const cepValidado= await buscarCEP();
  if (!cepValido) return;

  const criarUsuario1 = async() => {
    try {
    const A= await addDoc(collection(db,'UserForms'),{Nome,Letra, Consoante, Substantivo, Preposiçao, Adjetivo, Conjunçao
 ,cep, Logradouro, Localidade });

  navigation.navigate("ListUsers",{
    produto:{
      id:docRef.id,
      Nome, Letra, Consoante, Substantivo, Preposiçao, Adjetivo, Conjunçao
      ,cep,Logradouro,Localidade,Resultado
    }  });} catch(error){
   console.log('Erro ao adicionar:',error)
    }
  };
  Navigation.navigate('ListUsers', { produto: seusDadosDoFormulario });




  return (
    <ScrollView style={styles.container}>
     <Text style={styles.UP}>Pagina de Listagem Pricipal</Text>
     
      <Text style={styles.down}>Nome Completo</Text>
        <TextInput style={styles.nome} placeholder="digite seu nome completlo" value ={Nome} onChangeText={setNome}/>
       
      <Text  style={styles.down}>Telefone</Text>
          <TextInput style={styles.nome}  placeholder="digite seu telefone celular" value ={Letra} onChangeText={setLetra}/>
     
      <Text  style={styles.down}>CPF </Text>
          <TextInput style={styles.nome}  placeholder="digite seu CPF" value ={Consoante} onChangeText={setConsoante}/>
    
      <Text style={styles.down}> CEP</Text>
        <TextInput style={styles.nome}  placeholder="digite seu nome cep" value ={cep} onChangeText={setcep} keyboardType='numeric' maxlenght={8}/> 
        <TextInput style={styles.nome}  placeholder="digite todos os capos necessarios" value={Logradouro} onChangeText={setLogradouro} keyboardType="numeric"/>
        <TextInput style={styles.nome}  placeholder="digite todos os capos necessarios" value={Localidade} onChangeText={setLocalidade}/>
      <Button title="buscar cep" color="green" onPress={buscarCEP} />
      <Text style={styles.down}>Logradouro </Text>
          <TextInput style={styles.nome}  placeholder="digite seu logradouro" value ={Substantivo}onChangeText={setSubstantivo}/>
   
      <Text style={styles.down}>Bairro </Text>
          <TextInput style={styles.nome}  placeholder="digite seu bairro" value ={Preposiçao} onChangeText={setPreposiçao}/>
    
      <Text style={styles.down}>Cidade </Text>
          <TextInput style={styles.nome}  placeholder="digite seu cidade " value ={Adjetivo} onChangeText={setAdjetivo}/>
    
      <Text style={styles.down}>UF(estado)  </Text>
          <TextInput style={styles.nome}  placeholder="digite seu UF(estado)" value ={Conjunçao} onChangeText={setConjunçao}/>
      
    <Button title="Salvar dados" color="green" onPress={enviarDados} />
      <Button title="ir para pagina Listusers" onPress={() => Navigation.navigate("ListUsers")}/>
       </ScrollView>
  );
}}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  nome:{
    backgoundColor:"Gray",
    height:23,
    boderWidth:1,
    borderColor:'green',
    colorfont:"white",
    margin:10,
    fontsize:15,
    padding:10,
},
 up:{
  color:'green',
  fontize:25,
  fontWeight:'bold',
  textAling:'center',
  marginvertical:30,
},
down:{
 color:"purple",
fontize:25,
  fontWeight:'bold',
  textAling:'center',
  marginvertical:30,
  
},
});
 
