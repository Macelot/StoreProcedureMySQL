# StoreProcedureMySQL
Exemplo de Store procedure no MySQL
Tabelas de teste:

CREATE TABLE IF NOT EXISTS `venda_tela` (
  `idVenda` int(5) NOT NULL AUTO_INCREMENT,
  `idClienteFK` int(5) DEFAULT NULL,
  `numeroParcelas` int(4) DEFAULT NULL,
  `subTotal` decimal(15,2) DEFAULT NULL,
  `totalVenda` decimal(15,2) DEFAULT NULL,
  `idUsuarioFK` int(10) NOT NULL COMMENT 'chave primaria do colaborador que realizou ou cadastrou a ultima atualizacao',
  `dataCadastro` datetime NOT NULL COMMENT 'data de cadastro do registro',
  `dataUpdate` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'data da ultima atualizacao do registro',
  `ativo` char(1) NOT NULL COMMENT 'se o registro esta ativo ou nao',
  `versao` int(10) NOT NULL COMMENT 'usado para manter a integridade quando feitas alteracoes simultaneas.',
  PRIMARY KEY (`idVenda`)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;

CREATE TABLE IF NOT EXISTS `itemVenda_rel` (
  `idItemVenda` int(11) NOT NULL AUTO_INCREMENT,
  `idVendaFK` int(11) DEFAULT NULL,
  `idProdutoFK` int(11) NOT NULL,
  `quantidadeVenda` int(11) NOT NULL,
  `valorItem` decimal(15,2) DEFAULT NULL,
  PRIMARY KEY (`idItemVenda`)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8 AUTO_INCREMENT=4 ;


CREATE TABLE IF NOT EXISTS `produto_tela` (
  `idProduto` bigint(20) NOT NULL AUTO_INCREMENT,
  `descricao` varchar(50) CHARACTER SET latin1 NOT NULL,
  `imagem` longblob,
  `preco` decimal(10,2) NOT NULL,
  `obs` varchar(150) CHARACTER SET latin1 NOT NULL,
  `tipoProdutoFK` bigint(20) DEFAULT NULL,
  `subTipoProdutoFK` int(11) NOT NULL,
  `corProdutoFK` int(11) NOT NULL,
  `quantidadeVendida` int(11) NOT NULL,
  `quantidadeComprada` int(11) NOT NULL,
  `idUsuarioFK` int(10) NOT NULL,
  `dataUpdate` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `versao` int(11) DEFAULT NULL,
  `ativo` tinyint(4) NOT NULL,
  `dataCadastro` datetime NOT NULL,
  `quantidade` int(11) DEFAULT NULL,
  PRIMARY KEY (`idProduto`)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8 AUTO_INCREMENT=16 ;


Procedure para inserir um item na tabela itemVenda_rel. Os parâmetros são: 
idVendaFK – id da venda 
idProdutoFK – id do produto
quantidadeVendida – quantidade vendida
valorItem – valor de cada produto

DELIMITER //
CREATE PROCEDURE frameWork.inseriritem_venda_tela(
in idVenda int, 
in idProduto int, 
in quantidade int, 
in valor decimal (15,2) ) 
BEGIN 
INSERT INTO itemVenda_rel (
    idVendaFK,
    idProdutoFK,
    quantidadeVenda,
    valorItem
    ) VALUES (
    idVenda,
    idProduto,
    quantidade,
    valor );
END //
DELIMITER ;

Para executar
call inseriritem_venda_tela(1,5,3,5.00);


