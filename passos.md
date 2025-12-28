
# Spring Batch Envio de E-mail Automático

- criar os bancos e tabelas 
- criar as classes do domínio (domain)
- configurar a conexão do banco no application.properties
- configurar os data sources (config)
	- DataSourceConfig

```java
@Configuration
public class DataSourceConfig {
	
	//esse é a configuração do banco padrão, no caso do spring batch para gravar os meta dados
	@Primary
	@Bean
	@ConfigurationProperties(prefix = "spring.datasource")
	public DataSource springDS() {
		return DataSourceBuilder.create().build();
	}
	
	//esse é a configuração do banco da aplicação, seguido da configuração
	//do gerencador de transação
	@Bean
	@ConfigurationProperties(prefix = "app.datasource")
	public DataSource appDS() {
		return DataSourceBuilder.create().build();
	}
	
	@Bean
	public PlatformTransactionManager transactionManagerApp(@Qualifier("appDS") DataSource dataSource) {
		return new DataSourceTransactionManager(dataSource);
	}

}
```

- criar o job para enviar a notificação (job)
- criar o step, definir a quantidade de chunks e o gerenciador de transação (step)
	- definir no step o reader, processor e writer
	- implementar o read (reader) e informar o data source (banco de dados) da aplicação
		- implementar o rowmapper
	- implementar o processamento (processor)
		- importar a dependência do sendgrid	
	- implementar o writing (writer)
- criar o agendamento de envio de e-mails usando o Quartz com Spring Batch
	- criar o JobLauncher - responsável pela execução do Job
