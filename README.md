import React, { useState } from 'react';

function CadastroFazenda() {
  const [nome, setNome] = useState('');
  const [areaCultivada, setAreaCultivada] = useState('');
  const [tipoSafra, setTipoSafra] = useState('1a_safra');
  const [latitude, setLatitude] = useState('');
  const [longitude, setLongitude] = useState('');

  const getLocation = () => {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          setLatitude(position.coords.latitude);
          setLongitude(position.coords.longitude);
        },
        (error) => {
          console.error('Erro ao obter localização:', error);
        }
      );
    } else {
      alert('Geolocalização não é suportada pelo seu navegador.');
    }
  };

  const handleSubmit = async () => {
    const dadosFazenda = {
      nome,
      areaCultivada,
      tipoSafra,
      latitude,
      longitude,
    };
    // Enviar dados para o backend
    const response = await fetch('/api/fazendas', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(dadosFazenda),
    });
    if (response.ok) {
      alert('Fazenda cadastrada com sucesso!');
    } else {
      alert('Erro ao cadastrar fazenda.');
    }
  };

  return (
    <form onSubmit={(e) => { e.preventDefault(); handleSubmit(); }}>
      <label>
        Nome da Fazenda:
        <input type="text" value={nome} onChange={(e) => setNome(e.target.value)} required />
      </label>
      <label>
        Área Cultivada (ha):
        <input type="number" value={areaCultivada} onChange={(e) => setAreaCultivada(e.target.value)} required />
      </label>
      <label>
        Tipo de Safra:
        <select value={tipoSafra} onChange={(e) => setTipoSafra(e.target.value)} required>
          <option value="1a_safra">Algodão 1ª Safra</option>
          <option value="2a_safra">Algodão 2ª Safra</option>
        </select>
      </label>
      <label>
        Localização:
        <button type="button" onClick={getLocation}>Capturar Localização</button>
        <input type="text" value={latitude} placeholder="Latitude" readOnly />
        <input type="text" value={longitude} placeholder="Longitude" readOnly />
      </label>
      <button type="submit">Salvar</button>
    </form>
  );
}

    export default CadastroFazenda;