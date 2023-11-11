using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace AppCrud
{
    public partial class FrmPrincipal : Form
    {
        public FrmPrincipal()
        {
            InitializeComponent();
        }

        private void btnSair_Click(object sender, EventArgs e)
        {
            Environment.Exit(0);
        }

        private void FrmPrincipal_Load(object sender, EventArgs e)
        {
            Pessoa pessoa = new Pessoa();
            List<Pessoa> pes = pessoa.listapessoa();
            dgvPessoa.DataSource = pes;
            btnExcluir.Enabled = false; 
            btnEditar.Enabled = false;
            this.ActiveControl = txtNome;
        }

        private void btnInserir_Click(object sender, EventArgs e)
        {
            try 
            {
                if (txtNome.Text == "" || txtIdade.Text == "")
                {
                    MessageBox.Show("Por favor, preencha todos os campos!");
                    txtNome.Text = "";
                    txtIdade.Text = "";
                    this.ActiveControl = txtNome;
                    return;
                }
                Pessoa pessoa = new Pessoa();
                if (pessoa.RegistroRepetido(txtNome.Text, txtIdade.Text) == true)
                {
                    MessageBox.Show("Pessoa já existe em nossa base de dados!");
                    txtNome.Text = "";
                    txtIdade.Text = "";
                    this .ActiveControl = txtNome;
                }
                else
                {
                    pessoa.Inserir(txtNome.Text, txtIdade.Text);
                    MessageBox.Show("Pessoa cadastrada com suceso!");
                    List<Pessoa> pes = pessoa.listapessoa();
                    dgvPessoa.DataSource = pes;
                    txtNome.Text = "";
                    txtIdade.Text = "";
                    this.ActiveControl = txtNome;
                }
            } 
            catch (Exception er)
            {
                MessageBox.Show(er.Message);
            }

        }

        private void btnLocalizar_Click(object sender, EventArgs e)
        {
            try
            {
                if (txtId.Text == "")
                {
                    MessageBox.Show("Por favor, digite um ID para localizar");
                    return;
                }
                else
                {
                    int Id = Convert.ToInt32(txtId.Text.Trim());
                    Pessoa pessoa = new Pessoa();
                    pessoa.Localiza(Id);
                    txtNome.Text = pessoa.nome;
                    txtIdade.Text = pessoa.idade;
                    if (txtNome.Text != null)
                    {
                        btnEditar.Enabled = true;
                        btnExcluir.Enabled = true;
                    }
                }
            }
            catch (Exception er)
            {
                MessageBox.Show(er.Message);
            }
        }

        private void btnEditar_Click(object sender, EventArgs e)
        {
            try
            {
                if (txtId.Text == "")
                {
                    MessageBox.Show("Por favor, digite um Id para editar");
                    return;
                }
                else
                {
                    int Id = Convert.ToInt32(txtId.Text.Trim());
                    Pessoa pessoa = new Pessoa();
                    pessoa.Atualizar(Id, txtNome.Text, txtIdade.Text);
                    MessageBox.Show("Pessoa atualizada com sucesso!");
                    List<Pessoa> pes = pessoa.listapessoa();
                    dgvPessoa.DataSource = pes;
                    txtNome.Text = "";
                    txtIdade.Text = "";
                    this.ActiveControl = txtNome;
                }
            }
            catch (Exception er)
            {
                MessageBox.Show(er.Message);
            }
        }

        private void btnExcluir_Click(object sender, EventArgs e)
        {
            try
            {
                int Id = Convert.ToInt32(txtId.Text.Trim());
                Pessoa pessoa = new Pessoa();
                pessoa.Excluir(Id);
                MessageBox.Show("Pessoa exlcuida com sucesso!");
                List<Pessoa> pes = pessoa.listapessoa();
                dgvPessoa.DataSource = pes;
                txtId.Text = "";
                txtNome.Text = "";
                txtIdade.Text = "";
                this.ActiveControl = txtNome;
            }
            catch (Exception er)
            {
                MessageBox.Show (er.Message);
            }
        }

        private void dgvPessoa_CellContentClick(object sender, DataGridViewCellEventArgs e)
        {
            if (e.RowIndex >= 0)
            {
                DataGridViewRow row = this.dgvPessoa.Rows[e.RowIndex];
                this.dgvPessoa.Rows[e.RowIndex].Selected = true;
                txtId.Text = row.Cells[0].Value.ToString();
                txtNome.Text = row.Cells[1].Value.ToString();
                txtIdade.Text = row.Cells[2].Value.ToString();
            }
            btnEditar.Enabled = true;
            btnExcluir.Enabled = true;
        }
    }
}

