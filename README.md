# ABC-PATENTE-
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/// @title Patente da NSC INC
/// @author Lucas Januário do Nascimento
/// @notice Todos os direitos reservados
contract PatenteNSCINC {
    struct Patente {
        uint256 numero;
        string titulo;
        string descricao;
        address proprietario;
        uint256 dataRegistro;
    }

    mapping(uint256 => Patente) public patentes;
    uint256 public totalPatentes;

    event PatenteRegistrada(
        uint256 indexed numero,
        string titulo,
        address indexed proprietario,
        uint256 dataRegistro
    );

    modifier apenasProprietario(uint256 _numero) {
        require(msg.sender == patentes[_numero].proprietario, "Somente o proprietario pode executar esta acao");
        _;
    }

    function registrarPatente(
        string memory _titulo,
        string memory _descricao
    ) public {
        totalPatentes++;
        patentes[totalPatentes] = Patente(
            totalPatentes,
            _titulo,
            _descricao,
            msg.sender,
            block.timestamp
        );

        emit PatenteRegistrada(totalPatentes, _titulo, msg.sender, block.timestamp);
    }

    function transferirPatente(uint256 _numero, address _novoProprietario) public apenasProprietario(_numero) {
        patentes[_numero].proprietario = _novoProprietario;
    }

    function obterInformacoesPatente(uint256 _numero) public view returns (
        uint256 numero,
        string memory titulo,
        string memory descricao,
        address proprietario,
        uint256 dataRegistro
    ) {
        Patente memory p = patentes[_numero];
        return (p.numero, p.titulo, p.descricao, p.proprietario, p.dataRegistro);
    }

    // Função para obter informações sobre o proprietário e direitos reservados
    function obterInformacoesProprietario() public pure returns (string memory, string memory) {
        return ("Lucas Januário do Nascimento", "Todos os direitos reservados.");
    }
}
